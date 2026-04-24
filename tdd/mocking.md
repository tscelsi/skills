# When to Mock

Mock at **system boundaries** only:

- External APIs (payment, email, etc.)
- Databases (sometimes - prefer test DB)
- Time/randomness
- File system (sometimes)

Don't mock:

- Your own classes/modules
- Internal collaborators
- Anything you control

## Designing for Mockability

At system boundaries, design interfaces that are easy to mock:

**1. Use dependency injection**

Pass external dependencies in rather than creating them internally:

```python
# Easy to mock
def process_payment(order: Order, payment_client: PaymentClient) -> ChargeResult:
    return payment_client.charge(order.total_cents)


# Hard to mock
def process_payment(order: Order) -> ChargeResult:
    client = StripeClient(api_key=os.environ["STRIPE_API_KEY"])
    return client.charge(order.total_cents)
```

**2. Prefer SDK-style interfaces over generic fetchers**

Create specific functions for each external operation instead of one generic function with conditional logic:

```python
# GOOD: each method is independently mockable
class PaymentsApi:
    def get_customer(self, customer_id: str) -> dict: ...
    def list_invoices(self, customer_id: str) -> list[dict]: ...
    def create_invoice(self, payload: dict) -> dict: ...


# BAD: generic method forces conditional logic in test doubles
class GenericApi:
    def request(self, endpoint: str, method: str, payload: dict | None = None) -> dict: ...
```

The SDK approach means:
- Each mock returns one specific shape
- No conditional logic in test setup
- Easier to see which endpoints a test exercises
- Type safety per endpoint

## Python-specific Mocking Guidance

- Prefer fakes/stubs for your own boundaries; use `unittest.mock`/`pytest-mock` for external systems.
- Patch where the object is looked up, not where it is defined.
- Avoid autospeccing internal collaborators just to assert call choreography.
