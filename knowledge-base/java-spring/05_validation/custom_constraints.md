# Custom Constraints

Custom constraints are useful when a validation rule is part of the API
contract and is reused across requests.

Use them carefully. Not every rule needs an annotation.

## Example Constraint

```java
@Target({ ElementType.FIELD, ElementType.PARAMETER })
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = OrderCodeValidator.class)
public @interface ValidOrderCode {
    String message() default "Invalid order code";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

```java
class OrderCodeValidator implements ConstraintValidator<ValidOrderCode, String> {
    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        return value == null || value.matches("ORD-[0-9]{6}");
    }
}
```

```java
public record FindOrderRequest(
    @ValidOrderCode String orderCode
) {
}
```

## Good Use Cases

Custom constraints are reasonable for:

- reusable identifier formats;
- strict code formats;
- cross-field DTO checks;
- reusable boundary-level constraints.

## Avoid Database-Dependent Validators

Validators that query repositories often blur validation and business behavior.
For example, "email must be unique" usually belongs in the service layer because
it depends on persistence and race conditions.

## Key Idea

Write custom constraints for reusable boundary rules. Keep stateful business
checks in application services.
