# **The Essence of TDD**

**— Vertical-Slice First. Invariant-Driven. A Design Discovery Loop. —**

---

## Preface: Why This Document Exists

Test-Driven Development (TDD) is widely misunderstood.

“Write tests first.”
“A process for quality assurance.”
“Red → Green → Refactor.”

These statements aren’t wrong.

But they are insufficient.

---

The core of TDD is deeper.

**TDD is a practice for converging an uncertain, fragile reality into a design that actually holds.**

This document is not a prescription.
It is a lens for observing how design assumptions settle—or fail to.

---

This document is written for engineers who:

* Practice TDD but feel something is “off.”
* Write tests, yet find their design no longer advances.
* Maintain immaculate unit tests, only to see systems fail at integration.
* Use AI to generate tests, but sense that their confidence is shallow.

---

How to read this document:

1. **Chapters 1–7** describe TDD as a design discovery practice.
2. **Chapters 8–10** diagnose “TDD Cosplay,” where tests exist but learning stops.
3. **Chapter 11** walks through a concrete e-commerce example.
4. **Chapters 12–13** operationalize the Invariant Queue and discuss AI-era TDD.
5. **Chapters 14–16** define boundaries: growth stages, environments, and failure modes.

---

## Table of Contents

1. The Premise Shift
2. A Definition of Real TDD
3. Why Vertical Slice First
4. Integration Tests, Redefined
5. Invariants and Constraints as a Priority Queue
6. Where Unit Tests Actually Fit
7. What This TDD Gives You
8. The Danger of “TDD Cosplay”
9. Diagnostic Questions
10. The Definition of Failure
11. Practice Example: E-Commerce Order Flow
12. Operating the Invariant Queue
13. TDD in the Age of AI
14. The Four Stages of Sculpting
15. The “Stacked Development” Trap by Environment
16. Where Vertical Slice TDD Fails

Afterword
Appendix: Success Metrics

---

## 1. The Premise Shift

### The Common Misconception

```text
✗ Unit tests accumulate correctness.
✗ Integration tests merely verify connectivity.
✗ TDD is just the “Red → Green → Refactor” sequence.
```

This framing is incomplete.

Why?
**Because correct parts do not guarantee a correct system.**

---

### The Premise of This Document

```text
✓ What breaks is rarely implementation detail.
  It is assumptions, constraints, and meaning.

✓ What matters is not “does the feature run,”
  but “are invariants preserved end-to-end?”

✓ Therefore, TDD is not primarily a testing technique.
  It is a design discovery technique.
```

---

## 2. A Definition of Real TDD

**TDD is a design discovery loop that settles invariants, one vertical slice at a time.**

---

### Red

A declaration that the current world does not yet hold.

### Green

A signal that one specific assumption is now settled.
You no longer need to doubt it.

### Refactor

Observation time.
A deliberate pause to discover the next constraint likely to break.

---

## 3. Why Vertical Slice First

### The Limits of Horizontal Slicing

```text
✗ Measures only local correctness.
✗ Misses boundaries, state transitions, and timing.
✗ Creates false confidence about system invariants.
```

**Horizontal-slice TDD produces tested components, but rarely confirmed systems.**

---

### The Essence of a Vertical Slice

A vertical slice is the smallest cross-section that includes:

1. **Business Meaning** — a real outcome.
2. **Invariants / Kill Conditions** — what must never break.
3. **End-to-End Consistency** — a path crossing all relevant layers.

If it isn’t a vertical slice, you cannot answer:

> *“Does this world actually hold?”*

---

## 4. Integration Tests, Redefined

Integration tests are not “unit tests, but bigger.”

**They are the entry point for design discovery.**

* Written first
* Painfully red
* Maximally informative

They do not test connectivity.
They test whether meaning and constraints survive the journey.

---

## 5. Invariants and Constraints as a Priority Queue

### Not a List, but a Queue

Invariants and constraints are not tasks.
They are **unconfirmed design hypotheses**.

They must be managed as a **priority queue**, not a checklist.

---

### The Three Axes

```text
1. Lethality — If this breaks, does the system fail?
2. Irreversibility — How hard is this to change later?
3. Uncertainty — Do we need to learn this early?
```

---

### Operating the Queue

1. Confirm one invariant.
2. Observe how the system changed.
3. If new constraints appear, re-rank.

**The queue is never “done.”
Learning reshuffles it continuously.**

---

## 6. Where Unit Tests Actually Fit

Unit tests matter.

But they are not the protagonist.

Their role is to act as stabilizers — training wheels — that make vertical slices safe and fast.

The order is always:

```text
Integration → Unit
```

---

## 7. What This TDD Gives You

```text
✓ Early failure
✓ Explainable failure
✓ Decisions locked into executable form
✓ Abstractions driven by pressure, not taste
```

The result is not comfort.
**It is earned confidence.**

---

## 8. The Danger of “TDD Cosplay”

It is easy to imitate the form of TDD without practicing its substance.

We call this state **TDD Cosplay**.

```text
✓ Tests exist
✓ Rituals are followed
✓ CI is green
```

Yet design truth has not advanced.

---

## 9. Diagnostic Questions

If you cannot answer these, you may be cosplaying:

* What assumption became “no longer doubtful” when this test passed?
* Which invariant did this green actually settle?
* Why was this the next constraint to test?
* Does this test pierce a real vertical slice?

If the only answer is *“I feel safer,”* design has stalled.

---

## 10. The Definition of Failure

> **Tests increase, but settled assumptions do not.**

Conversely:

> **Few tests that settle a fatal invariant are success.**

---

## 11. Practice Example: E-Commerce Order Flow

Let's feel the difference with concrete code.
You can copy and paste these examples to verify the behavior yourself.

---

### Scenario

E-commerce flow:
1. Add item to cart.
2. Check stock.
3. Process payment.
4. Confirm order.

---

### Invariant List

```text
1. Stock >= Order Quantity
2. Order only confirmed after successful payment
3. Duplicate items increment quantity
4. Cancellation restores stock
5. Stock reservation happens before payment
6. Reservation expires in 15 mins
```

---

### Prioritization (3 Axes)

| Invariant | Fatal? | Irreversible? | Uncertain? | Priority |
|---|---|---|---|---|
| Stock >= Order Qty | ◎ | ○ | ◎ | **1** |
| Payment Required for Order | ◎ | ◎ | ○ | **2** |
| Reserve before Payment | ◎ | ○ | ◎ | **3** |
| Reservation Expiry | ○ | △ | ◎ | 4 |
| Cancel restores stock | ○ | △ | ○ | 5 |

---

### The Anti-Pattern (Horizontal Slice)

First, the **wrong way**.
These unit tests all pass perfectly, yet the system contains a critical bug on integration.

#### 1. CartService.spec.ts (Unit)

```typescript
// src/cart/cart.service.spec.ts
import { CartService } from './cart.service';

describe('CartService', () => {
  it('should add item to cart', () => {
    const cart = new CartService();
    cart.addItem({ productId: 'P1', quantity: 2 });
    expect(cart.getItems()).toContainEqual(
      expect.objectContaining({ productId: 'P1', quantity: 2 })
    );
  });
});
```

#### 2. InventoryService.spec.ts (Unit)

```typescript
// src/inventory/inventory.service.spec.ts
import { InventoryService } from './inventory.service';

describe('InventoryService', () => {
  it('should return true if stock is sufficient', async () => {
    const mockDb = { getStock: jest.fn().mockResolvedValue(10) };
    const inventory = new InventoryService(mockDb);
    
    const result = await inventory.checkStock('P1', 5);
    expect(result).toBe(true);
  });
});
```

#### 3. PaymentService.spec.ts (Unit)

```typescript
// src/payment/payment.service.spec.ts
import { PaymentService } from './payment.service';

describe('PaymentService', () => {
  it('should process payment successfully', async () => {
    const mockGateway = { charge: jest.fn().mockResolvedValue('tx_123') };
    const payment = new PaymentService(mockGateway);
    
    const result = await payment.process(1000);
    expect(result.success).toBe(true);
  });
});
```

#### The Reveal: Integration Failure

When combined, the following code allows **negative stock** under concurrency.

```typescript
// main.ts (The Reality)
// User A and User B click "Buy" simultaneously.

// Thread A
const stockA = await inventory.checkStock('P1', 1); // OK (Stock 1)

// Thread B (Interruption)
const stockB = await inventory.checkStock('P1', 1); // OK (Stock 1)

// Thread A
await payment.process(1000);
await order.confirm(); // Stock becomes 0

// Thread B
await payment.process(1000);
await order.confirm(); // 💥 Stock becomes -1 (Invariant Violated)
```

**The parts were correct. The system was broken.**

---

### The Correct Approach (Vertical Slice)

Now, the approach that **confirms invariants**.

#### Step 1: Vertical Slice Integration Test

We test the most fatal invariant "Stock >= Order Quantity" first, **including concurrency simulation**.

```typescript
// test/integration/order-process.spec.ts
import { TestDb } from '../utils/test-db';
import { CheckoutService } from '../../src/checkout/checkout.service';

describe('Order Process Integration (Vertical Slice)', () => {
  let db: TestDb;
  let checkout: CheckoutService;

  beforeEach(() => {
    db = new TestDb();
    checkout = new CheckoutService(db);
  });

  // Invariant: "Stock >= Order Quantity" must hold even under concurrency
  it('should prevent order completion if stock is depleted during process', async () => {
    // 1. Arrange: Only 1 item in stock
    await db.inventory.set('P1', 1);

    // 2. Act: Add to cart and start checkout (before reservation)
    const cart = new Cart();
    cart.addItem({ productId: 'P1', quantity: 1 });
    const session = await checkout.startCheckout(cart);

    // 3. Concurrency Simulation:
    // Someone else snatches the stock here
    await db.inventory.forceReduce('P1', 1); // Stock becomes 0

    // 4. Assert: Should fail on checkout completion
    // (A horizontal implementation would pass here and cause negative stock)
    await expect(
      checkout.completeCheckout(session)
    ).rejects.toThrow('Insufficient Stock');

    // 5. Invariant Assertion: Stock must never be negative
    const finalStock = await db.inventory.get('P1');
    expect(finalStock).toBeGreaterThanOrEqual(0);
  });
});
```

**🔴 RED: Breaks immediately (no implementation or no locking).**

---

#### Step 2: Minimal Implementation (Green)

Implement the bare minimum to protect the invariant.

```typescript
// src/checkout/checkout.service.ts
export class CheckoutService {
  constructor(private db: Database) {}

  async startCheckout(cart: Cart) {
    // Just create session or do nothing
    return { sessionId: 'sess_123', ...cart };
  }

  async completeCheckout(session: Session) {
    return this.db.transaction(async (tx) => {
      // 1. Lock and Check Stock (FOR UPDATE)
      const currentStock = await tx.inventory.getWithLock(session.productId);

      // 2. Guard: Invariant Check
      if (currentStock < session.quantity) {
        throw new Error('Insufficient Stock');
      }

      // 3. Reduce Stock
      await tx.inventory.reduce(session.productId, session.quantity);

      // 4. Charge and Confirm
      await this.paymentService.charge(session.amount);
      await tx.orders.create(session);
    });
  }
}
```

**✅ GREEN: Invariant Confirmed.**

The invariant "Stock >= Order Quantity" is no longer a guess. It is a **settled fact**.

---

#### Step 3: Refactor & Observe

Now that it's green, new constraints appear:

- What if payment fails? Do we rollback stock?
- Is the lock holding too long?

Add these to the **Priority Queue** and proceed to the next loop.

---

#### Step 4: Next Vertical Slice Test

Next, we test "Order confirmed only on Successful Payment."

```typescript
describe('Order Process Integration', () => {
  it('should not create order if payment fails', async () => {
    // Arrange
    await db.inventory.set('P1', 10);
    const cart = new Cart();
    cart.addItem({ productId: 'P1', quantity: 5 });
    
    // Force Payment Failure
    paymentGateway.setShouldFail(true);
    
    const checkoutService = new CheckoutService(db, paymentGateway);
    const session = await checkoutService.startCheckout(cart);
    
    // Act & Assert
    await expect(
      checkoutService.completeCheckout(session)
    ).rejects.toThrow('Payment failed');
    
    // Invariant: No Order Created
    const orders = await db.orders.findBySession(session.sessionId);
    expect(orders).toHaveLength(0);
    
    // Invariant: Stock Released
    const stock = await db.inventory.get('P1');
    expect(stock).toBe(10); // Rolled back
  });
});
```

**🔴 RED: Fails (Stock release not implemented).**

---

#### Step 5: Implementation

```typescript
class CheckoutService {
  async completeCheckout(session: CheckoutSession) {
    const reservation = await this.getReservation(session.reservationId);
    
    if (!reservation || reservation.expiresAt < Date.now()) {
      // Release Stock on Expiry
      await this.releaseReservation(reservation);
      throw new Error('Reservation expired');
    }
    
    try {
      // Charge Payment
      await this.paymentService.process(session.amount);
    } catch (error) {
      // Release Stock on Failure
      await this.releaseReservation(reservation);
      throw error;
    }
    
    // Confirm Reservation
    await this.confirmReservation(reservation);
    
    // Confirm Order
    return new Order(session);
  }
  
  private async releaseReservation(reservation: Reservation) {
    await this.db.transaction(async (tx) => {
      // Restore Stock
      await tx.inventory.increment(
        reservation.productId,
        reservation.quantity
      );
      await tx.reservations.delete(reservation.id);
    });
  }
}
```

**✅ GREEN: New Invariants Confirmed.**

---

## 12. Operating the Invariant Queue

* List invariants
* Rank by lethality, irreversibility, uncertainty
* Confirm one
* Re-rank

Never clear the queue.
Discovery never stops.

---

## 13. TDD in the Age of AI

AI accelerates TDD — and TDD Cosplay.

AI can generate code and tests.
**It cannot choose which assumptions matter.**

Invariant selection, prioritization, and observation remain human responsibilities.

---

## 14. The Four Stages of Sculpting

1. **Skeleton** — invariants and failure paths
2. **Translation** — UI/UX as a projection
3. **Reality Check** — exposure to real users
4. **Decision** — continue, pivot, or stop

Only Stage 1 makes the later stages safe.

---

## 15. The “Stacked Development” Trap by Environment

Enterprise, SI, and startup environments differ —
but the root failure is the same:

> **Believing the wrong premise too early.**

Vertical Slice TDD exists to surface that mistake early.

---

## 16. Where Vertical Slice TDD Fails

Vertical Slice TDD fails when:

* There is no uncertainty
* Speed is the only metric
* Certainty is politically rejected
* It is treated as process, not decision

A discovery tool is dangerous where discovery is forbidden.

---

## Afterword

TDD is not a way to write tests.

**It is a discipline for defining the premises on which your system holds.**

Start with a vertical slice.
Welcome the pain.
Settle one fatal assumption.

That is **Real TDD**.

This is not a recipe, a framework, or a new flavor of TDD.
If you are looking for a checklist, this document will disappoint you.

It exists to help you notice when learning has stopped —
and to give you a way to restart it.

Some readers have asked how to recognize progress in practice.
A non-normative note is available in `appendix/SUCCESS_METRICS.md`.

---

© 2025 Felix Asher
Licensed under CC BY-ND 4.0

Vertical-Slice TDD™ is a conceptual framework described in this document.
