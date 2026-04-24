# Interface Design for Testability

Good interfaces make testing natural:

1. **Accept dependencies, don't create them**

   ```python
   # Testable
   def process_order(order: Order, payment_gateway: PaymentGateway) -> Receipt:
       ...


   # Hard to test
   def process_order(order: Order) -> Receipt:
       gateway = StripeGateway()
       ...
   ```

2. **Return results, don't produce side effects**

   ```python
   # Testable
   def calculate_discount(cart: Cart) -> Discount:
       ...


   # Hard to test
   def apply_discount(cart: Cart) -> None:
       cart.total_cents -= cart.discount_cents
   ```

3. **Small surface area**
   - Fewer methods = fewer tests needed
   - Fewer params = simpler test setup

4. **Use typed contracts at boundaries**
   - Define `Protocol` interfaces for external collaborators.
   - Keep domain function signatures explicit and stable.

5. **Prefer immutable value objects for domain data**
   - Use `@dataclass(frozen=True)` when practical to reduce incidental state mutation.
