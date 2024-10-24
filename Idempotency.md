## Idempotency

**Idempotency** is a mathematical concept that describes an operation that can be performed multiple times without changing the result. In the context of software systems, an idempotent operation is one that produces the same outcome regardless of how many times it is executed.

**Example:**

Imagine a banking system where a customer transfers $100 from their checking account to their savings account. If the transfer operation is idempotent, it should produce the same result whether it is executed once, twice, or a hundred times. That is, the customer's accounts should only be updated once, regardless of the number of requests.

**Why is Idempotency Important?**

Idempotency is crucial in distributed systems, especially when dealing with network failures or retries. If an operation is not idempotent, it can lead to unintended consequences, such as double charges, incorrect data updates, or system inconsistencies.

**Architectural Patterns Related to Idempotency**

Several architectural patterns are closely related to idempotency:

1. **[CQRS](CQRS.md) (Command Query Responsibility Segregation):**
   * Separates read and write operations.
   * Commands can be made idempotent by using unique identifiers and tracking their execution status.

2. **Event Sourcing:**
   * Stores the history of changes to a system as a sequence of events.
   * Idempotency can be ensured by replaying events without side effects.

3. **Saga Pattern:**
   * A distributed transaction pattern that uses a chain of local transactions to achieve a global result.
   * Idempotency is essential for handling failures and retries in sagas.

**Implementing Idempotency**

There are several ways to implement idempotency in a software system:

### **1. Unique Identifiers:** Assign a unique identifier to each request and store information about its execution status.

```java
public class OrderService {
    private Map<String, Order> orders = new HashMap<>();

    public Order createOrder(String orderId, OrderRequest request) {
        if (orders.containsKey(orderId)) {
            // Order already exists, return it
            return orders.get(orderId);
        }

        // Create a new order
        Order order = new Order(orderId, request);
        orders.put(orderId, order);
        return order;
    }
}
```

In this example, a unique `orderId` is used to identify each order. If an order with the same ID already exists, it is returned instead of creating a new one.

### **2. Token-Based Approaches:** Use tokens to validate requests and prevent duplicates.

```java
public class OrderService {
    private Map<String, String> tokens = new HashMap<>();

    public Order createOrder(String token, OrderRequest request) {
        if (tokens.containsKey(token)) {
            // Token already exists, return an error
            throw new IllegalArgumentException("Token already used");
        }

        // Create a new order and store the token
        Order order = new Order(request);
        tokens.put(token, order.getId());
        return order;
    }
}
```

Here, a unique token is generated for each request. If the token is already used, an error is returned to prevent duplicate orders.

### **3. Optimistic Concurrency Control:** Assume that conflicts are rare and use version numbers to detect and resolve them.

```java
@Entity
public class Order {
    @Id
    @GeneratedValue
    private Long id;

    @Version
    private Long version;

    // ... other fields

    public Order update(OrderRequest request, Long version) {
        if (!this.version.equals(version)) {
            throw new OptimisticLockException("Version mismatch");
        }

        // Update the order
        // ...

        return this;
    }
}
```

In this example, the `@Version` annotation is used to track the version of an entity. If the version provided in the update request doesn't match the current version, an `OptimisticLockException` is thrown.

### **4. Pessimistic Concurrency Control:** Lock resources to prevent concurrent access and ensure consistency.

```java
@Transactional
public Order createOrder(OrderRequest request) {
    Order order = new Order(request);
    entityManager.persist(order);
    return order;
}
```

By using a transactional context, the database will automatically lock the entity during the transaction, preventing concurrent updates.

By implementing idempotency, you can build more reliable and resilient distributed systems that can handle failures and retries gracefully.
