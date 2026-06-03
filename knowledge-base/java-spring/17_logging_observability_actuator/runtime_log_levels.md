# Runtime Log Levels

The actuator `loggers` endpoint can inspect and change logger levels while the
application is running.

This is useful during incidents when restarting the service or changing a
deployment is too slow.

## Enable The Endpoint

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health,info,loggers
```

Expose this endpoint only to trusted operators.

## Inspect A Logger

```http
GET /actuator/loggers/com.example.orders
```

The response shows the configured and effective levels for that logger.

## Temporarily Enable Debug

```http
POST /actuator/loggers/com.example.orders
Content-Type: application/json

{
  "configuredLevel": "DEBUG"
}
```

## Reset The Logger

```http
POST /actuator/loggers/com.example.orders
Content-Type: application/json

{
  "configuredLevel": null
}
```

## Operational Rule

Runtime debug logging should be temporary. Leave a note in the incident or
support ticket explaining which logger was changed and when it was reset.

## Key Idea

Runtime log level changes are a production support tool. They are powerful, but
they need access control and discipline.
