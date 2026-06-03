# Domain and Application Exceptions

Application exceptions should express meaningful use-case failures.

They should not be thin wrappers around database errors or generic messages such
as `SomethingWentWrongException`.

## Good Exception Names

```java
OrderNotFoundException
OrderCancellationNotAllowedException
DuplicateCustomerEmailException
PaymentAuthorizationFailedException
```

These names describe the business or application condition.

## Example

```java
public class OrderCancellationNotAllowedException extends RuntimeException {
    private final Long orderId;
    private final String currentStatus;

    public OrderCancellationNotAllowedException(Long orderId, String currentStatus) {
        super("Order cannot be cancelled from status " + currentStatus);
        this.orderId = orderId;
        this.currentStatus = currentStatus;
    }

    public Long orderId() {
        return orderId;
    }

    public String currentStatus() {
        return currentStatus;
    }
}
```

The handler can map this to `409 Conflict` because the request conflicts with
the current resource state.

## Keep Technical Exceptions Internal

Do not expose repository, JDBC, Hibernate, or downstream client exception names
as API error codes. Translate them to application-level meaning when possible.

## Key Idea

Application exceptions should describe the failed use case in language the
service owns.
