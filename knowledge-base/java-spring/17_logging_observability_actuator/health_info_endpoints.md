# Health and Info Endpoints

Health endpoints are used by load balancers, orchestrators, uptime checks, and
support teams. They should report whether the application can serve traffic.

## Readiness and Liveness

```yaml
management:
  endpoint:
    health:
      probes:
        enabled: true
      show-details: when_authorized
```

Common meanings:

- liveness: the process is alive and should not be restarted;
- readiness: the service is ready to receive traffic.

Do not put expensive checks into health endpoints. A health check that overloads
the database during an incident makes the incident worse.

## Custom Health Indicator

```java
@Component
public class PaymentProviderHealthIndicator implements HealthIndicator {
    private final PaymentProviderClient paymentProviderClient;

    public PaymentProviderHealthIndicator(PaymentProviderClient paymentProviderClient) {
        this.paymentProviderClient = paymentProviderClient;
    }

    @Override
    public Health health() {
        if (paymentProviderClient.isAvailable()) {
            return Health.up().build();
        }

        return Health.down()
            .withDetail("provider", "payment-gateway")
            .build();
    }
}
```

Keep custom indicators fast and predictable. Prefer short timeouts.

## Info Endpoint

```yaml
management:
  info:
    env:
      enabled: true

info:
  app:
    name: order-service
    version: 1.4.2
```

The info endpoint is useful for deployment metadata, but it should not expose
secrets, database URLs, internal tokens, or customer data.

## Key Idea

Health should answer whether the service can operate. Info should identify what
is deployed. Neither endpoint should become a data dump.
