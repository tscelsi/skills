# Good and Bad Tests

## Good Tests

**Integration-style**: Test through real interfaces, not mocks of internal parts.

```python
# GOOD: tests observable behavior
def test_user_can_checkout_with_valid_cart(payment_gateway_stub):
    cart = Cart()
    cart.add(Product("sku-1", price_cents=1200))

    result = checkout(cart=cart, payment_gateway=payment_gateway_stub)

    assert result.status == "confirmed"
```

Characteristics:

- Tests behavior users/callers care about
- Uses public API only
- Survives internal refactors
- Describes WHAT, not HOW
- One logical assertion per test

## Bad Tests

**Implementation-detail tests**: Coupled to internal structure.

```python
# BAD: tests internal collaboration details
def test_checkout_calls_payment_service_process(mocker):
    payment_service = mocker.Mock()

    checkout(cart=Cart(), payment_gateway=payment_service)

    payment_service.process.assert_called_once()
```

Red flags:

- Mocking internal collaborators
- Testing private methods
- Asserting on call counts/order
- Test breaks when refactoring without behavior change
- Test name describes HOW not WHAT
- Verifying through external means instead of interface

```python
# BAD: bypasses interface to verify persistence details
def test_create_user_saves_to_database(db_connection):
    create_user(name="Alice")

    row = db_connection.execute(
        "SELECT name FROM users WHERE name = ?",
        ("Alice",),
    ).fetchone()

    assert row is not None


# GOOD: verifies behavior through public interface
def test_create_user_makes_user_retrievable():
    user = create_user(name="Alice")

    retrieved = get_user(user.id)

    assert retrieved.name == "Alice"
```
