## Transactions

**Transactions** are a fundamental concept in database systems and distributed applications. They ensure that a series of operations are executed as a single unit, either all succeeding or all failing. This guarantees data consistency and integrity.

### ACID Properties of Transactions

* **Atomicity:** A transaction is treated as an indivisible unit. Either all operations within the transaction are executed successfully, or none are.
* **Consistency:** A transaction must leave the database in a consistent state.
* **Isolation:** Transactions should not interfere with each other.
* **Durability:** The effects of a committed transaction must be persistent and survive system failures.

### Architectural Patterns Related to Transactions

1. **Two-Phase Commit (2PC):**
   * A distributed transaction protocol that coordinates multiple participants to ensure consistency.
   * Involves a coordinator and participants.
   * Can be complex and prone to deadlocks.

2. **Saga Pattern:**
   * A distributed transaction pattern that uses a chain of local transactions to achieve a global result.
   * Each local transaction is compensated for in case of failure.
   * More flexible than 2PC but can be complex to implement.

3. **Event Sourcing:**
   * Stores the history of changes to a system as a sequence of events.
   * Transactions can be implemented by applying events in a specific order.
   * Provides better auditability and recoverability.

4. **Optimistic Concurrency Control (OCC):**
   * Assumes conflicts are rare and checks for conflicts before committing a transaction.
   * Can be more efficient than pessimistic concurrency control.

Example of Optimistic Concurrency Control (OCC) using Java:

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

@Service
public class OrderService {
    @Autowired
    private OrderRepository orderRepository;

    public Order updateOrder(Long orderId, OrderRequest request) {
        Order order = orderRepository.findById(orderId).orElseThrow(
                () -> new EntityNotFoundException("Order not found")
        );

        order.update(request, order.getVersion());
        orderRepository.save(order);
        return order;
    }
}
```

In this example:

   * The `@Version` annotation on the `Order` entity is used to track its version and indicates that the field should be used for optimistic locking. Spring Data JPA will automatically check the version before updating the entity and throw an OptimisticLockException if there is a conflict.
   * The `update` method checks if the provided version matches the current version. If not, an `OptimisticLockException` is thrown.
   * The `OrderService` updates the order and saves it to the repository.
   * The `@OptimisticLocking` annotation can be applied at the class level to enable optimistic locking for all entities within the class.

5. **Pessimistic Concurrency Control (PCC):**
   * Locks resources to prevent concurrent access.
   * Can be less efficient but provides stronger guarantees.

Example of Pessimistic Concurrency Control (PCC) using Java:

```java
@Service
public class OrderService {
    @Autowired
    private OrderRepository orderRepository;

    @Transactional
    public Order updateOrder(Long orderId, OrderRequest request) {
        Order order = orderRepository.findById(orderId).orElseThrow(
                () -> new EntityNotFoundException("Order not found")
        );

        // Update the order
        // ...

        return order;
    }
}
```

In this example:

   * By using the `@Transactional` annotation on a method, Spring will automatically create a transactional context that ensures that all operations within the method are executed as a single unit. This effectively provides pessimistic concurrency control.
   * The `@Transactional` annotation on the `updateOrder` method ensures that the entire operation is executed within a transaction, providing pessimistic concurrency control. The `@Version` annotation on the `Order` entity enables optimistic locking, which can be used in combination with pessimistic locking if necessary.

**Key Points:**

- **OCC** is generally more efficient than PCC, as it avoids unnecessary locking. However, it can lead to retries in case of conflicts.
- **PCC** provides stronger guarantees of data consistency but can be less efficient.
- The choice between OCC and PCC depends on the specific requirements of your application, such as the expected frequency of conflicts and the importance of data consistency.

### Example: Banking Transaction

In a banking system, a transaction might involve transferring funds from one account to another. This transaction should be atomic, ensuring that both accounts are updated correctly or not at all. If the transaction fails, the system should be able to roll back the changes, preventing data inconsistencies.

**Choosing the Right Pattern:**

The choice of transaction pattern depends on factors such as:

* **Consistency requirements:** How critical is data consistency?
* **Performance needs:** How important is transaction speed?
* **Complexity:** How complex is the system and its interactions with other systems?
