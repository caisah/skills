# Simple Code Examples

These examples show the kind of simplification this skill should prefer: local, behavior-preserving changes that reduce mental overhead.

## 1. Replace Nested Branches With Guard Clauses

Selected pillars:

- Cyclomatic Complexity Reduction
- Linear Control Flow

Before:

```javascript
function canShip(order) {
  if (order) {
    if (order.isPaid) {
      if (!order.isCancelled) {
        return true;
      }
    }
  }

  return false;
}
```

After:

```javascript
function canShip(order) {
  if (!order?.isPaid) return false;
  if (order.isCancelled) return false;
  return true;
}
```

## 2. Remove Trivial Indirection

Selected pillars:

- Indirection Depth Control

Before:

```javascript
function processUser(input) {
  return saveUser(input);
}
```

After:

```javascript
saveUser(input);
```

## 3. Make State Explicit

Selected pillars:

- State-Space Minimization

Before:

```javascript
const view = {
  isLoading: false,
  hasError: false,
  isEmpty: true,
};
```

After:

```javascript
const view = {
  status: "idle",
};
```

## 4. Separate Abstraction Levels

Selected pillars:

- Abstraction Level Uniformity
- Signal-to-Noise Ratio (SNR)

Before:

```javascript
function createInvoice(items, discounts, rawCustomerId) {
  const customerId = rawCustomerId.trim().toLowerCase();
  const total = items.reduce((sum, item) => sum + item.price, 0);
  if (discounts.vip) {
    return saveInvoice({ customerId, total: total * 0.8 });
  }

  return saveInvoice({ customerId, total });
}
```

After:

```javascript
function createInvoice(items, discounts, rawCustomerId) {
  const customerId = normalizeCustomerId(rawCustomerId);
  const total = calculateInvoiceTotal(items, discounts);
  return saveInvoice({ customerId, total });
}
```

## 5. Do Not Extract a Helper That Only Renames Complexity

Selected pillars:

- Indirection Depth Control

Before:

```javascript
function sendWelcomeEmail(user) {
  const recipient = buildRecipient(user);
  const message = buildWelcomeMessage(user);
  return dispatchWelcomeEmail(recipient, message);
}

function dispatchWelcomeEmail(recipient, message) {
  return emailClient.send(recipient, message);
}
```

After:

```javascript
function sendWelcomeEmail(user) {
  const recipient = buildRecipient(user);
  const message = buildWelcomeMessage(user);
  return emailClient.send(recipient, message);
}
```

Reason:

`dispatchWelcomeEmail()` adds no policy, validation, transformation, or reuse value. The extra name hides a direct call without reducing complexity.

## 6. Do Not Add an Abstraction With No Immediate Payoff

Selected pillars:

- Information Hiding (Surface Area)
- Signal-to-Noise Ratio (SNR)

Before:

```javascript
function formatPrice(amount) {
  return `$${amount.toFixed(2)}`;
}
```

After:

```javascript
function formatPrice(amount) {
  return `$${amount.toFixed(2)}`;
}
```

Avoid replacing it with:

```javascript
class PriceFormattingService {
  format(amount) {
    return `$${amount.toFixed(2)}`;
  }
}
```

Reason:

The service abstraction adds ceremony, indirection, and lifecycle surface without solving a current complexity or reuse problem.

## 7. Make Data Flow Explicit

Selected pillars:

- Data Flow Explicitness
- Referential Transparency

Before:

```javascript
function createSession(user) {
  const startedAt = Date.now();
  auditLogger.info("session_started", user.id);
  return sessionStore.create(user.id, startedAt);
}
```

After:

```javascript
function createSession(user, clock, logger, store) {
  const startedAt = clock.now();
  logger.info("session_started", user.id);
  return store.create(user.id, startedAt);
}
```

Reason:

The function no longer depends on hidden global services. Its inputs and side effects are visible at the call site.

## 8. Reduce Change Amplification

Selected pillars:

- Change Amplification Minimization
- Pattern Symmetry

Before:

```javascript
function canRefund(order) {
  return order.status === "paid" && order.total < 1000;
}

function showRefundButton(order) {
  return order.status === "paid" && order.total < 1000;
}

function createRefundAuditEvent(order) {
  if (order.status === "paid" && order.total < 1000) {
    return { type: "refund_allowed", orderId: order.id };
  }

  return null;
}
```

After:

```javascript
function isRefundAllowed(order) {
  return order.status === "paid" && order.total < 1000;
}

function canRefund(order) {
  return isRefundAllowed(order);
}

function showRefundButton(order) {
  return isRefundAllowed(order);
}

function createRefundAuditEvent(order) {
  if (!isRefundAllowed(order)) return null;
  return { type: "refund_allowed", orderId: order.id };
}
```

Reason:

The refund rule now lives in one place, so future policy changes require one edit instead of several coordinated edits.

## 9. Shorten Variable Live Range

Selected pillars:

- Variable Span & Live Range
- Locality of Reference

Before:

```javascript
function publishReport(user, data) {
  const report = buildReport(data);
  const isAllowed = user.role === "admin";

  logAccess(user);
  notifyReviewers(user);

  if (!isAllowed) throw new Error("Forbidden");
  return saveReport(report);
}
```

After:

```javascript
function publishReport(user, data) {
  if (user.role !== "admin") throw new Error("Forbidden");

  logAccess(user);
  notifyReviewers(user);

  const report = buildReport(data);
  return saveReport(report);
}
```

Reason:

Values are introduced closer to where they matter, so the reader tracks less state across unrelated lines.
