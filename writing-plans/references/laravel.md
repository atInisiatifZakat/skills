# Laravel — Template & Contoh

## Anatomy Pseudocode Spec

```php
/**
 * [CONSTRAINT] <business rule>
 * [IDEMPOTENT] <opsional>
 *
 * @deps    ServiceA, RepositoryB (diinject via constructor)
 * @reads   table_a, table_b
 * @writes  table_c
 * @emits   XxxEvent{field1, field2}
 *
 * @throws  XxxException   <kondisi>
 * @throws  YyyException   <kondisi>
 */
public function methodName(RequestDTO $dto): ResultType
{
    // GUARD: <kondisi> → throw new XxxException(...)
    //
    // FETCH: $data via Repository::findOrFail($dto->id)
    // GUARD: <post-fetch kondisi> → throw new XxxException(...)
    //
    // FETCH: $items via Repository::findManyByIds($dto->ids) — eager, batch
    // GUARD: item tidak ditemukan → throw new XxxNotFoundException($id)
    //
    // TRANSFORM: $dto + $items → Collection<OutputType>
    //   - detail kalkulasi
    //
    // GUARD: <business validation> → throw new XxxException(...)
    //
    // DB::transaction(function () use (...) {
    //   - Model::create([...])
    //   - $entity->relation()->createMany($items)
    //   - Repository::decrementStock($items)
    // })
    //
    // dispatch(new XxxEvent($field1, $field2))
    //
    // return $result
}
```

## DTO Shape

Deklarasi DTO **sebelum** service task — agent tidak perlu tebak struktur input.

```php
final class CreateXxxDTO
{
    public function __construct(
        public readonly string $userId,
        public readonly string $idempotencyKey,
        /** @var array<XxxItemDTO> */
        public readonly array  $items,
    ) {}
}

final class XxxItemDTO
{
    public function __construct(
        public readonly string $productId,
        public readonly int    $qty,
    ) {}
}
```

---

## Contoh Lengkap: CreateOrder

### Task 1: DTO & Domain Types

**Files:**
- Create: `app/Order/DTOs/CreateOrderDTO.php`
- Create: `app/Order/DTOs/OrderItemDTO.php`
- Create: `app/Order/Exceptions/OrderException.php`

- [ ] **Step 1: Tulis DTOs dan exceptions**

```php
<?php
namespace App\Order\DTOs;

final class CreateOrderDTO
{
    public function __construct(
        public readonly string $userId,
        public readonly string $idempotencyKey,
        /** @var array<OrderItemDTO> */
        public readonly array $items,
    ) {}
}

final class OrderItemDTO
{
    public function __construct(
        public readonly string $productId,
        public readonly int $qty,
    ) {}
}
```

```php
<?php
namespace App\Order\Exceptions;

use RuntimeException;

class EmptyItemsException extends RuntimeException {}
class UnverifiedUserException extends RuntimeException {}
class InsufficientStockException extends RuntimeException {
    public function __construct(public readonly string $productId) {
        parent::__construct("Insufficient stock for product: {$productId}");
    }
}
class ProductNotFoundException extends RuntimeException {
    public function __construct(public readonly string $productId) {
        parent::__construct("Product not found: {$productId}");
    }
}
```

- [ ] **Step 2: Compile check** `php artisan test --filter=nothing`
- [ ] **Step 3: Commit** `git commit -m "feat(order): add DTOs and exceptions"`

---

### Task 2: CreateOrder Service

**Spec:**
```php
/**
 * [CONSTRAINT] Order hanya bisa dibuat oleh verified user dengan sufficient stock.
 *              Idempotent berdasarkan idempotency_key.
 * [IDEMPOTENT] true — duplikat request return existing order
 *
 * @deps    UserRepository, ProductRepository, OrderRepository
 * @reads   users, products
 * @writes  orders, order_items, idempotency_records
 * @emits   OrderCreated{orderId, userId, totalAmount}
 *
 * @throws  EmptyItemsException       items kosong
 * @throws  UnverifiedUserException   user.status != verified
 * @throws  ProductNotFoundException  productId tidak ada
 * @throws  InsufficientStockException stock < qty
 */
public function createOrder(CreateOrderDTO $dto): Order
{
    // GUARD: $dto->items kosong → throw new EmptyItemsException()
    //
    // GUARD: idempotency_key sudah ada → return existingOrder
    //
    // FETCH: $user via UserRepository::findOrFail($dto->userId)
    // GUARD: $user->status !== 'verified' → throw new UnverifiedUserException()
    //
    // FETCH: $products via ProductRepository::findManyByIds($itemIds) — eager, batch
    // GUARD: count($products) !== count($dto->items) → throw ProductNotFoundException
    //
    // TRANSFORM: $dto->items + $products → Collection<OrderItem>
    //   - subtotal = item->qty * product->price
    //   - totalAmount = sum(subtotals)
    //
    // GUARD: per item, $product->stock < $item->qty → throw InsufficientStockException($productId)
    //
    // DB::transaction(function () {
    //   - Order::create([userId, totalAmount, status => 'pending'])
    //   - $order->items()->createMany($orderItems)
    //   - ProductRepository::decrementStock($dto->items)
    //   - IdempotencyRecord::create([key, orderId])
    // })
    //
    // dispatch(new OrderCreated($order->id, $dto->userId, $totalAmount))
    //
    // return $order
}
```

**Files:**
- Create: `app/Order/Services/OrderService.php`
- Test: `tests/Unit/Order/OrderServiceTest.php`

- [ ] **Step 1: Tulis failing tests**

```php
<?php
namespace Tests\Unit\Order;

use Tests\TestCase;
use App\Order\Services\OrderService;
use App\Order\DTOs\{CreateOrderDTO, OrderItemDTO};
use App\Order\Exceptions\{EmptyItemsException, UnverifiedUserException,
    InsufficientStockException, ProductNotFoundException};
use App\Models\{User, Order};

class OrderServiceTest extends TestCase
{
    public function test_throws_when_items_empty(): void
    {
        $svc = $this->makeService();
        $this->expectException(EmptyItemsException::class);
        $svc->createOrder(new CreateOrderDTO('user-1', 'key-1', []));
    }

    public function test_returns_existing_on_duplicate_idempotency_key(): void
    {
        $existing = Order::factory()->make(['id' => 'order-existing']);
        $svc = $this->makeService(idempotencyResult: $existing);

        $result = $svc->createOrder(new CreateOrderDTO(
            'user-1', 'key-dup', [new OrderItemDTO('p1', 1)]
        ));
        $this->assertEquals('order-existing', $result->id);
    }

    public function test_throws_when_user_not_verified(): void
    {
        $svc = $this->makeService(user: User::factory()->make(['status' => 'unverified']));
        $this->expectException(UnverifiedUserException::class);
        $svc->createOrder(new CreateOrderDTO(
            'user-1', 'key-1', [new OrderItemDTO('p1', 1)]
        ));
    }

    public function test_throws_when_product_not_found(): void
    {
        $svc = $this->makeService(
            user: User::factory()->make(['status' => 'verified']),
            products: collect([]),
        );
        $this->expectException(ProductNotFoundException::class);
        $svc->createOrder(new CreateOrderDTO(
            'user-1', 'key-1', [new OrderItemDTO('p-missing', 1)]
        ));
    }

    public function test_throws_when_insufficient_stock(): void
    {
        $product = (object)['id' => 'p1', 'price' => 1000, 'stock' => 2];
        $svc = $this->makeService(
            user: User::factory()->make(['status' => 'verified']),
            products: collect([$product]),
        );
        $this->expectException(InsufficientStockException::class);
        $svc->createOrder(new CreateOrderDTO(
            'user-1', 'key-1', [new OrderItemDTO('p1', 5)]
        ));
    }

    public function test_success_dispatches_event(): void
    {
        \Event::fake();
        $product = (object)['id' => 'p1', 'price' => 5000, 'stock' => 10];
        $svc = $this->makeService(
            user: User::factory()->make(['status' => 'verified']),
            products: collect([$product]),
        );
        $order = $svc->createOrder(new CreateOrderDTO(
            'user-1', 'k1', [new OrderItemDTO('p1', 2)]
        ));
        $this->assertEquals(10000, $order->total_amount);
        \Event::assertDispatched(\App\Order\Events\OrderCreated::class);
    }

    private function makeService(...$overrides): OrderService { /* mock setup */ }
}
```

- [ ] **Step 2: Run → pastikan FAIL**
```bash
php artisan test tests/Unit/Order/OrderServiceTest.php
# Expected: Error — OrderService not found
```

- [ ] **Step 3: Implementasi** (ikuti spec di atas)
```php
public function createOrder(CreateOrderDTO $dto): Order
{
    // implementasi mengikuti pseudocode spec di atas
}
```

- [ ] **Step 4: Run → pastikan PASS**
```bash
php artisan test tests/Unit/Order/OrderServiceTest.php
# Expected: 6 passed
```

- [ ] **Step 5: Commit**
```bash
git add app/Order/Services/ tests/Unit/Order/
git commit -m "feat(order): implement createOrder with idempotency"
```
