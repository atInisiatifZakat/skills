# Go — Template & Contoh

## Anatomy Pseudocode Spec

```go
// [CONSTRAINT] <business rule>
// [IDEMPOTENT] true — <kondisi idempotency>
//
// deps  : InterfaceA, InterfaceB
// reads : table_a, table_b
// writes: table_c
// emits : event.name{field1, field2}
func (s *XxxService) MethodName(ctx context.Context, req RequestType) (*Result, error) {
    // GUARD: <kondisi> → return nil, ErrXxx
    //
    // FETCH: data from Repo by id
    // GUARD: <post-fetch kondisi> → return nil, ErrXxx
    //
    // FETCH: items from Repo by ids (batch)
    // GUARD: item tidak ditemukan → return nil, &ErrXxxNotFound{ID: id}
    //
    // TRANSFORM: input + data → OutputType
    //   - kalkulasi detail
    //
    // GUARD: <business validation> → return nil, ErrXxx
    //
    // ATOMIC (tx):
    //   - Repo.Insert(tx, ...)
    //   - Repo.Update(tx, ...)
    //   - Repo.SaveIdempotencyKey(tx, key, id)
    //
    // EMIT: XxxEvent{field1, field2}
    //
    // return result, nil
}
```

## Interface / Repository

Deklarasi interface **sebelum** service task — agent tidak perlu tebak method names.

```go
type XxxRepository interface {
    // Komentar singkat apa yang dilakukan, dan contract-nya
    FindByID(ctx context.Context, id string) (*Xxx, error)
    FindManyByIDs(ctx context.Context, ids []string) ([]*Xxx, error)
    CreateWithItems(ctx context.Context, tx *sql.Tx, entity Xxx, items []Item) (*Xxx, error)
    FindByIdempotencyKey(ctx context.Context, key string) (*Xxx, error)
    SaveIdempotencyKey(ctx context.Context, tx *sql.Tx, key, entityID string) error
}
```

---

## Contoh Lengkap: CreateOrder

### Task 1: Domain Types & Errors

**Files:**
- Create: `internal/order/domain.go`

- [ ] **Step 1: Tulis types dan errors**

```go
package order

import (
    "errors"
    "fmt"
    "time"
)

type OrderStatus string
const (
    OrderStatusPending   OrderStatus = "pending"
    OrderStatusConfirmed OrderStatus = "confirmed"
)

type Order struct {
    ID          string
    UserID      string
    Items       []OrderItem
    TotalAmount int64
    Status      OrderStatus
    CreatedAt   time.Time
}

type OrderItem struct {
    ProductID string
    Qty       int
    Subtotal  int64
}

type CreateOrderRequest struct {
    UserID         string
    IdempotencyKey string
    Items          []OrderItemRequest
}

type OrderItemRequest struct {
    ProductID string
    Qty       int
}

var (
    ErrEmptyItems        = errors.New("order: items cannot be empty")
    ErrUnverifiedUser    = errors.New("order: user not verified")
    ErrInsufficientStock = errors.New("order: insufficient stock")
)

type ErrProductNotFound struct{ ProductID string }
func (e *ErrProductNotFound) Error() string {
    return fmt.Sprintf("order: product %s not found", e.ProductID)
}
```

- [ ] **Step 2: Compile check**
```bash
go build ./internal/order/...
# Expected: no errors
```

- [ ] **Step 3: Commit**
```bash
git add internal/order/domain.go
git commit -m "feat(order): add domain types and errors"
```

---

### Task 2: Repository Interface

**Files:**
- Create: `internal/order/repository.go`

- [ ] **Step 1: Tulis interfaces**

```go
package order

import (
    "context"
    "database/sql"
)

type UserRepository interface {
    FindByID(ctx context.Context, id string) (*User, error)
}

type ProductRepository interface {
    FindManyByIDs(ctx context.Context, ids []string) ([]*Product, error)
    DecrementStock(ctx context.Context, tx *sql.Tx, items []OrderItemRequest) error
}

type OrderRepository interface {
    CreateWithItems(ctx context.Context, tx *sql.Tx, order Order, items []OrderItem) (*Order, error)
    FindByIdempotencyKey(ctx context.Context, key string) (*Order, error)
    SaveIdempotencyKey(ctx context.Context, tx *sql.Tx, key, orderID string) error
}
```

- [ ] **Step 2: Compile check** `go build ./internal/order/...`
- [ ] **Step 3: Commit** `git commit -m "feat(order): add repository interfaces"`

---

### Task 3: CreateOrder Service

**Spec:**
```go
// [CONSTRAINT] Order hanya bisa dibuat oleh verified user dengan sufficient stock.
//              Satu order per kombinasi (userID + idempotencyKey).
// [IDEMPOTENT] true — duplikat request return existing order
//
// deps  : UserRepo, ProductRepo, OrderRepo, EventBus
// reads : users, products
// writes: orders, order_items, idempotency_records
// emits : order.created{orderID, userID, totalAmount}
func (s *OrderService) CreateOrder(ctx context.Context, req CreateOrderRequest) (*Order, error) {
    // GUARD: req.Items kosong → return nil, ErrEmptyItems
    // GUARD: req.IdempotencyKey sudah ada → return existingOrder, nil
    //
    // FETCH: user from UserRepo by req.UserID
    // GUARD: user.Status != Verified → return nil, ErrUnverifiedUser
    //
    // FETCH: products from ProductRepo by req.Items[*].ProductID (batch)
    // GUARD: ada product tidak ditemukan → return nil, &ErrProductNotFound{id}
    //
    // TRANSFORM: req.Items + products → []OrderItem
    //   - subtotal = item.Qty * product.Price
    //   - totalAmount = sum(subtotals)
    //
    // GUARD: product.Stock < item.Qty → return nil, ErrInsufficientStock
    //
    // ATOMIC (tx):
    //   - OrderRepo.CreateWithItems(tx, order, items)
    //   - ProductRepo.DecrementStock(tx, items)
    //   - OrderRepo.SaveIdempotencyKey(tx, key, orderID)
    //
    // EMIT: order.created{orderID, userID, totalAmount}
    //
    // return order, nil
}
```

**Files:**
- Create: `internal/order/service.go`
- Test: `internal/order/service_test.go`

- [ ] **Step 1: Tulis failing tests**

```go
package order_test

import (
    "context"
    "testing"
    "github.com/stretchr/testify/assert"
    "github.com/stretchr/testify/require"
)

func TestCreateOrder_EmptyItems(t *testing.T) {
    svc := newTestService(t)
    _, err := svc.CreateOrder(context.Background(), CreateOrderRequest{
        UserID: "user-1", IdempotencyKey: "key-1", Items: nil,
    })
    require.ErrorIs(t, err, ErrEmptyItems)
}

func TestCreateOrder_IdempotentDuplicate(t *testing.T) {
    svc := newTestService(t)
    existing := &Order{ID: "order-existing"}
    svc.orderRepo.(*mockOrderRepo).idempotencyResult = existing

    result, err := svc.CreateOrder(context.Background(), CreateOrderRequest{
        UserID: "u1", IdempotencyKey: "key-dup",
        Items: []OrderItemRequest{{ProductID: "p1", Qty: 1}},
    })
    require.NoError(t, err)
    assert.Equal(t, "order-existing", result.ID)
}

func TestCreateOrder_UnverifiedUser(t *testing.T) {
    svc := newTestService(t)
    svc.userRepo.(*mockUserRepo).user = &User{ID: "u1", Status: UserStatusUnverified}

    _, err := svc.CreateOrder(context.Background(), CreateOrderRequest{
        UserID: "u1", IdempotencyKey: "k1",
        Items: []OrderItemRequest{{ProductID: "p1", Qty: 1}},
    })
    require.ErrorIs(t, err, ErrUnverifiedUser)
}

func TestCreateOrder_ProductNotFound(t *testing.T) {
    svc := newTestService(t)
    svc.userRepo.(*mockUserRepo).user = &User{ID: "u1", Status: UserStatusVerified}
    svc.productRepo.(*mockProductRepo).products = []*Product{}

    _, err := svc.CreateOrder(context.Background(), CreateOrderRequest{
        UserID: "u1", IdempotencyKey: "k1",
        Items: []OrderItemRequest{{ProductID: "p-missing", Qty: 1}},
    })
    var notFound *ErrProductNotFound
    require.ErrorAs(t, err, &notFound)
    assert.Equal(t, "p-missing", notFound.ProductID)
}

func TestCreateOrder_InsufficientStock(t *testing.T) {
    svc := newTestService(t)
    svc.userRepo.(*mockUserRepo).user = &User{ID: "u1", Status: UserStatusVerified}
    svc.productRepo.(*mockProductRepo).products = []*Product{
        {ID: "p1", Price: 1000, Stock: 2},
    }
    _, err := svc.CreateOrder(context.Background(), CreateOrderRequest{
        UserID: "u1", IdempotencyKey: "k1",
        Items: []OrderItemRequest{{ProductID: "p1", Qty: 5}},
    })
    require.ErrorIs(t, err, ErrInsufficientStock)
}

func TestCreateOrder_Success_EmitsEvent(t *testing.T) {
    svc := newTestService(t)
    svc.userRepo.(*mockUserRepo).user = &User{ID: "u1", Status: UserStatusVerified}
    svc.productRepo.(*mockProductRepo).products = []*Product{
        {ID: "p1", Price: 5000, Stock: 10},
    }
    order, err := svc.CreateOrder(context.Background(), CreateOrderRequest{
        UserID: "u1", IdempotencyKey: "k1",
        Items: []OrderItemRequest{{ProductID: "p1", Qty: 2}},
    })
    require.NoError(t, err)
    assert.Equal(t, int64(10000), order.TotalAmount)
    require.Len(t, svc.eventBus.(*mockEventBus).emitted, 1)
    assert.Equal(t, "order.created", svc.eventBus.(*mockEventBus).emitted[0].Name)
}
```

- [ ] **Step 2: Run → pastikan FAIL**
```bash
go test ./internal/order/... -run TestCreateOrder -v
# Expected: compile error — CreateOrder not defined
```

- [ ] **Step 3: Implementasi** (spec adalah contract — ikuti exactly, catat deviasi sebagai `// DEVIATION:`)
```go
func (s *OrderService) CreateOrder(ctx context.Context, req CreateOrderRequest) (*Order, error) {
    // implementasi mengikuti pseudocode spec di atas
}
```

- [ ] **Step 4: Run → pastikan PASS**
```bash
go test ./internal/order/... -run TestCreateOrder -v
# Expected: PASS — 6 tests
```

- [ ] **Step 5: Commit**
```bash
git add internal/order/service.go internal/order/service_test.go
git commit -m "feat(order): implement CreateOrder with idempotency"
```
