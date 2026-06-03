# `@Transactional` Basics

`@Transactional` tells Spring to run a method inside a transaction.

It is commonly placed on service methods because services coordinate complete
use cases.

## Basic Example

```java
@Service
class CustomerService {
    private final CustomerRepository customerRepository;

    @Transactional
    CustomerResponse register(RegisterCustomerCommand command) {
        Customer customer = Customer.create(command.email(), command.name());
        customerRepository.save(customer);
        return CustomerResponse.from(customer);
    }
}
```

## Class-Level Transaction

```java
@Service
@Transactional(readOnly = true)
class CustomerQueryService {
    CustomerResponse findById(Long id) {
        // read operation
    }
}
```

Method-level annotations can override class-level defaults.

## Avoid Controller Transactions

Putting transactions on controllers mixes HTTP boundary concerns with
persistence consistency. Keep transactions near application behavior.

## Transaction Manager

Spring uses a transaction manager appropriate for the persistence technology,
such as JPA or JDBC. Boot often auto-configures it when the data access stack is
present.

## Key Idea

Use `@Transactional` around service operations that need persistence consistency.
