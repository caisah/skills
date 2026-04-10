# Simple Code Pillars

Use these pillars as a reference when deciding which simplifications to apply. Treat thresholds as review signals, not rigid laws.

### 1. Working Memory Optimization

- **Principle**: Limit unique "chunks" of information to fit within human working memory capacity.
- **Constraint**: Prefer no more than 7 active variables or entities per function scope.
- **Technique**: **Extract Method**. Split a function into smaller, named abstractions.
- **Example**:
  ```javascript
  const total = calculateTotal(price, tax, discount);
  ```

### 2. Cyclomatic Complexity Reduction

- **Principle**: Minimize the number of independent execution paths.
- **Constraint**: Prefer cyclomatic complexity at or below 5 per function and nesting depth at or below 2.
- **Technique**: **Guard Clauses**. Replace nested `if/else` with early returns.
- **Example**:
  ```javascript
  if (!user?.isActive) return false;
  return true;
  ```

### 3. Signal-to-Noise Ratio (SNR)

- **Principle**: Maximize intent-carrying code while minimizing boilerplate and plumbing.
- **Constraint**: Prefer domain logic to dominate the function body rather than repetitive null checks, casting, or glue code.
- **Technique**: **Optional Chaining & Nullish Coalescing**.
- **Example**:
  ```javascript
  const city = user?.address?.city ?? "Unknown";
  ```

### 4. Referential Transparency

- **Principle**: Make functions predictable and free of hidden side effects when possible.
- **Constraint**: Keep calculation and query functions pure where practical.
- **Technique**: **Command-Query Separation (CQS)**.
- **Example**:
  ```javascript
  function add(a, b) {
    return a + b;
  }
  ```

### 5. Locality of Reference

- **Principle**: Keep declarations close to their first use.
- **Constraint**: Prefer variable declaration within 5 lines of first usage.
- **Technique**: **Late Declaration**.
- **Example**:
  ```javascript
  const config = loadConfig();
  if (config.enabled) { ... }
  ```

### 6. Information Hiding (Surface Area)

- **Principle**: Hide internal details from the outside world.
- **Constraint**: Keep most module internals private or unexported.
- **Technique**: **Encapsulation**.
- **Example**:
  ```javascript
  export function run() {
    initialize();
    step1();
    step2();
  }
  ```

### 7. Principle of Least Astonishment (POLA)

- **Principle**: Code should behave the way its name and context suggest.
- **Constraint**: Function names should clearly cover their main effects.
- **Technique**: **Semantic Renaming**.
- **Example**:
  ```javascript
  validateAndSaveUser();
  ```

### 8. Coupling Decoupling (Fan-out)

- **Principle**: Minimize how much a module knows about other modules.
- **Constraint**: Prefer no more than 5 external domain dependencies per module.
- **Technique**: **Dependency Injection**.
- **Example**:
  ```javascript
  const service = new BillingService(logger);
  ```

### 9. Linear Control Flow

- **Principle**: Keep reading order as top-to-bottom as possible.
- **Constraint**: Avoid `goto`, avoid `continue`, and keep async callback nesting shallow.
- **Technique**: **Async/Await Transformation**.
- **Example**:
  ```javascript
  const user = await fetchUser();
  const posts = await fetchPosts(user.id);
  ```

### 10. Halstead Volume (Vocabulary)

- **Principle**: Use the smallest set of unique operators and operands needed to solve the problem.
- **Constraint**: Prefer standard library operations over custom utilities for common tasks.
- **Technique**: **Standard Library Leverage**.
- **Example**:
  ```javascript
  values.includes(x);
  ```

### 11. High Cohesion (LCOM)

- **Principle**: Keep each module focused on one responsibility.
- **Constraint**: Class methods should center on shared state and one purpose.
- **Technique**: **Single Responsibility Principle (SRP)**.
- **Example**:
  ```javascript
  const report = reportGenerator.createPdf(user);
  ```

### 12. Pattern Symmetry

- **Principle**: Paired actions reduce cognitive orphans and leaks.
- **Constraint**: Every resource-acquiring function should have a mirrored teardown path.
- **Technique**: **Lifecycle Mirroring**.
- **Example**:
  ```javascript
  onConnect();
  onDisconnect();
  ```

### 13. Variable Span & Live Range

- **Principle**: Keep variables alive for the shortest practical time.
- **Constraint**: Prefer variable live range under 15 lines.
- **Technique**: **Block Scoping**.
- **Example**:
  ```javascript
  {
    const token = createToken(user);
    sendToken(token);
  }
  ```

### 14. Observability & Transparency

- **Principle**: Failures should be loud, clear, and informative.
- **Constraint**: No empty catch blocks; add context when logging or re-throwing errors.
- **Technique**: **Explicit Error Wrapping**.
- **Example**:
  ```javascript
  catch (err) { throw new AuthError('Failed to login', err); }
  ```

### 15. Change Amplification Minimization

- **Principle**: A single requirement change should require edits in as few places as possible.
- **Constraint**: Keep each business rule in one authoritative implementation whenever practical.
- **Technique**: **Single Source of Truth**.
- **Example**:
  ```javascript
  const eligible = discountPolicy.isEligible(user, cart);
  ```

### 16. State-Space Minimization

- **Principle**: Reduce the number of mutable states and reachable state combinations.
- **Constraint**: Prefer a small explicit set of meaningful states instead of many behavior-controlling flags.
- **Technique**: **Finite State Modeling**.
- **Example**:
  ```javascript
  const status = "idle";
  ```

### 17. Indirection Depth Control

- **Principle**: Keep the number of hops needed to understand behavior or data flow low.
- **Constraint**: Prefer resolving main behavior within 3 call layers from the entry point.
- **Technique**: **Inline Trivial Indirection**.
- **Example**:
  ```javascript
  return saveUser(input);
  ```

### 18. Abstraction Level Uniformity

- **Principle**: Each function should operate at one consistent level of abstraction.
- **Constraint**: Do not mix business rules, orchestration, and low-level parsing in the same block.
- **Technique**: **Single Level of Abstraction**.
- **Example**:
  ```javascript
  const total = calculateInvoiceTotal(items, discounts);
  ```

### 19. Data Flow Explicitness

- **Principle**: Inputs, outputs, and side effects should be visible at the point of use.
- **Constraint**: Core business functions should not depend on hidden globals, implicit singletons, or hidden mutable module state.
- **Technique**: **Dependency Injection**.
- **Example**:
  ```javascript
  const session = createSession(user, clock, logger);
  ```

### 20. Temporal Coupling Minimization

- **Principle**: Reduce dependencies on performing steps in a fragile or implicit order.
- **Constraint**: Prefer public APIs that require at most one mandatory setup step before safe use.
- **Technique**: **Make Invalid Order Impossible**.
- **Example**:
  ```javascript
  const client = createDatabaseClient(config);
  ```
