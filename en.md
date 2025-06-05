````markdown
## 1. Use HTTP Methods Appropriately

- **GET** — to retrieve resources (idempotent)
- **POST** — to create a new resource
- **PUT** — to update a resource (idempotent)
- **DELETE** — to delete a resource (idempotent)
- **PATCH** — to partially update a resource

## 2. Use HTTP Verbs Correctly

- **Incorrect:**

  ```http
  GET /products/123/delete
  ```
````

- **Correct:**

  ```http
  DELETE /products/123
  ```

## 3. Return Proper HTTP Status Codes

- **Incorrect:**

  ```http
  HTTP/1.1 200 OK
  Content-Type: application/json

  {
    "error": "Product not found",
    "status_code": 404
  }
  ```

- **Correct:**

  ```http
  HTTP/1.1 404 Not Found
  Content-Type: application/json

  {
    "error": "Product not found"
  }
  ```

## 4. Use Plural Nouns for Endpoints

```http
https://example.com/v1/users
https://example.com/v2/products
```

## 5. Avoid Verbs in Resource Names

- **Incorrect:**

  ```http
  GET /getProducts
  ```

- **Correct:**

  ```http
  GET /products
  ```

## 6. Structure Endpoint Hierarchy Clearly and Concisely

- **Incorrect:**

  ```http
  /reviews/123?productId=213
  /users/2/products/4452/.../..
  ```

- **Correct:**

  ```http
  /products/42/reviews/123
  /users/7
  ```

## 7. Version APIs Using URL Versioning

```http
GET /v1/users
GET /v2/users
```

## 8. Use Query Parameters for Filtering, Sorting, and Pagination

- **Incorrect:**

  ```http
  GET /products/expensive/sortedByPrice
  ```

- **Correct:**

  ```http
  GET /products?price=high&sort=price
  GET /products?category=new&sort=name&limit=10&offset=20
  ```

## 9. Specify Units and Date Formats When Using Parameters

- **Incorrect:**

  ```http
  GET /products?shippedOn=20230315
  GET /products?weight=5
  ```

- **Correct:**

  ```http
  GET /products?shippedOn=2023-03-15        # ISO 8601
  GET /products?weight=5&weightUnit=kg
  ```

## 10. Do Not Pass Sensitive Data in URLs; Use Request Body

- **Incorrect:**

  ```http
  https://example.com/login?password=myPassword
  https://example.com/register?email=unsafeMail
  ```

- **Correct:**
  Pass sensitive data (e.g., passwords, tokens, personal information) in the body of POST or PUT requests, not in query parameters or headers.

## 11. Always Wrap the Response in a Top-Level Object

- **Incorrect:**

  ```json
  [0, 1, 2, 3]
  ```

- **Correct:**

  ```json
  {
  	"data": [0, 1, 2, 3]
  }
  ```

## 12. Use ISO 8601 Format for Dates

```text
2025-06-05T14:30:00Z
```

## 13. Use Client-Friendly Terminology, Avoid Internal Terms

- **Incorrect:**

  ```http
  GET /cdc    # change-data-capture
  ```

- **Correct:**

  ```http
  GET /changes
  ```

## 14. Implement Filtering, Sorting, and Pagination

- **Incorrect:**

  ```http
  GET /products    # returns the full data set
  ```

- **Correct:**

  ```http
  GET /products?category=new&sort=name&limit=10&offset=20
  ```

## 15. Provide Clear and Informative Error Messages

- **Incorrect:**

  ```json
  {
  	"error": "Something went wrong"
  }
  ```

- **Correct:**

  ```json
  {
  	"error": "Product not found",
  	"status_code": 404
  }
  ```

## 16. Cache Frequently Requested or Static Data

- **Incorrect:**
  Always fetch data from the source.
- **Correct:**
  Use caching (Redis, in-memory, etc.) for frequently requested or rarely changing data.

## 17. Implement a Health Check Endpoint

```http
GET /health    # returns current API status
```

## 18. Use Rate Limiting and Throttling

- **Incorrect:**
  No limits on the number of requests.
- **Correct:**
  Limit the number of requests per time unit (e.g., 100 requests per minute) to prevent DDoS and abuse.

## 19. Use a Load Balancer

- **Incorrect:**
  Requests go directly to a single server.
- **Correct:**
  Requests are routed through a load balancer that distributes traffic across multiple service instances.

## 20. Write Quality Tests for Your API

- **Incorrect:**
  No automated tests.

- **Correct:**
  Implement:
  - Unit tests
  - Integration tests
  - End-to-end tests
  - Performance tests

## 21. Implement Monitoring and Alerts

- **Incorrect:**
  No monitoring tools in place.
- **Correct:**
  - Integrate monitoring tools (e.g., Prometheus, Zabbix)
  - Set up alerts (e.g., Slack, PagerDuty) for critical events (service downtime, high CPU/RAM, etc.)

## 22. Use HTTPS for Secure Connections

- **Incorrect:**

  ```http
  http://example.com/api
  ```

- **Correct:**

  ```http
  https://example.com/api    # SSL/TLS encryption
  ```

## 23. Do Not Expose Internal Information in Error Messages

- **Incorrect:**

  ```json
  {
  	"error": "NullPointerException at com.example.service.UserService",
  	"stack": "com.example.service.UserService.java:45"
  }
  ```

- **Correct:**

  ```json
  {
  	"error": "Internal server error"
  }
  ```

## 24. Implement Strong Authentication and Authorization

- **Authentication:**

  - Basic Auth (for simple use cases)
  - OAuth 2.0 (secure standard)
  - JWT (for stateless solutions)

- **Authorization:**

  - OAuth 2.0 with scopes/roles
  - OpenID Connect (for identity management)
  - RBAC/ABAC (role- or attribute-based access control)

## 25. Build Security into the API Design Process

- **Incorrect:**
  Add security after implementing functionality.
- **Correct:**
  Integrate security from the beginning (authentication, authorization, data validation, encryption).
