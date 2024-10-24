# Software Engineering Concepts

- [Big O Notation](#big-o-notation)
- [RESTful Web Services](#restful-web-services)
- [Idempotency](Idempotency.md)
- [Fail Tolerance](Fail-Tolerance.md)
- [Scalability](Scalability.md)
- System Design
  - Architectural Patterns
    - [CQRS](CQRS.md) - Command Query Responsibility Segregation

## Big O Notation

Big O notation is a way to describe the efficiency of algorithms, specifically in terms of time and space as the input size grows.

### Common Big O Notations

**O(1) \- Constant Time**: The runtime doesn't change with the input size.

Example: Accessing an element in an array.

```java
public static int getElement(int[] array, int index) {
    return array[index]; // O(1)
}
```

---

**O(n) - Linear Time:** The runtime increases linearly with the input size.

Example: Linear search in an array.

```java
public static int linearSearch(int[] array, int target) {
    for (int i = 0; i < array.length; i++) {
        if (array[i] == target) {
            return i; // O(1)
        }
    }
    return -1; // O(1)
}
```

---

**O(n^2) - Quadratic Time:** The runtime increases quadratically with the input size.

Example: Bubble sort algorithm.

```java
public static int linearSearch(int[] array, int target) {
    for (int i = 0; i < array.length; i++) {
        if (array[i] == target) {
            return i; // O(1)
        }
    }
    return -1; // O(1)
}
```

### Visualization of Big O

Think of Big O notation as a scale of efficiency:

1. O(1) is super fast.
2. O(n) is moderate.
3. O(n^2) is slower as the input size grows.

### Summary

Big O notation provides a high-level understanding of algorithm performance by focusing on the most significant factor that impacts runtime or space usage.

---

## RESTful Web Services

REST (Representational State Transfer) is an architecture style for designing networked applications using the principles of HTTP. Here are the key concepts:

### REST Key Concepts

1. **Resources** :
   * Represent data or services, identified by unique URLs.
   * Example:`https://api.example.com/users/123` represents the user with ID 123.
2. **HTTP Methods** :
   * **GET** : Retrieves data from a resource.
   * **POST** : Creates a new resource.
   * **PUT** : Updates an existing resource.
   * **DELETE** : Deletes a resource.
3. **Representations** :
   * Data can be represented in various formats, such as JSON or XML.
   * JSON Example:
     **json**Copiar

     ```
     {
       "id": 123,
       "name": "John Doe",
       "email": "john.doe@example.com"
     }
     ```
4. **Statelessness** :
   * Each client-server interaction must contain all the necessary information to process the request, without relying on stored context on the server.
5. **HATEOAS (Hypermedia As The Engine Of Application State)** :
   * Clients interact with the application through hypermedia provided by the server, allowing navigation through resources.
   * Example:
     **json**Copiar

     ```
     {
       "id": 123,
       "name": "John Doe",
       "links": [
         {"rel": "self", "href": "/users/123"},
         {"rel": "friends", "href": "/users/123/friends"}
       ]
     }
     ```
6. **Endpoints** :
   * URLs that represent resources and the possible operations.
   * Example:
     Copiar

     ```
     GET /users/ -> list of users
     POST /users/ -> create a new user
     GET /users/123 -> details of user with ID 123
     PUT /users/123 -> update user with ID 123
     DELETE /users/123 -> delete user with ID 123
     ```

### Advantages

* Simplicity: Using HTTP makes it easy to understand and use.
* Scalability: Stateless architecture allows for horizontal scaling.
* Flexibility: Supports various data formats like JSON, XML.

### Java (Spring Boot) Examples

#### Creating a RESTful Endpoint

```
@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        User user = userService.findById(id);
        return ResponseEntity.ok(user);
    }

    @PostMapping("/")
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User createdUser = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(createdUser);
    }
}
```
