# REST APIs

Documentation and practical study notes about **REST APIs**, **HTTP**, **JSON**, endpoints, request/response structure, CRUD operations, status codes, authentication, pagination, filtering, validation, error handling, API documentation, testing, and common technical interview topics.

This module is part of the `developer-knowledge-base` repository.

Recommended file path:

```text
backend/api-rest/README.md
```

---

## Objective

The objective of this module is to understand how REST APIs work and how to design, consume, document, test, and explain them professionally.

By the end of this module, the reader should be able to:

- Explain what an API is.
- Explain what REST means.
- Understand HTTP methods.
- Design RESTful endpoints.
- Send and receive JSON data.
- Use status codes correctly.
- Build CRUD operations.
- Understand authentication and authorization basics.
- Use pagination, filtering, sorting, and search.
- Handle errors using consistent response formats.
- Document APIs using OpenAPI / Swagger.
- Test APIs using Postman, cURL, browser tools, or automated tests.
- Prepare for REST API technical interviews.

---

## Version Coverage

> Last reviewed: 2026.  
> This README covers modern REST API fundamentals aligned with current HTTP semantics, JSON-based API design, JWT authentication, OpenAPI documentation practices, and common backend API design patterns.

Main practical references for this module include:

- HTTP Semantics based on RFC 9110.
- Current HTTP methods and status code documentation from MDN.
- OpenAPI Specification 3.x documentation practices.
- Common REST API design patterns used in modern backend development.

---

## Table of Contents

1. [Version Coverage](#version-coverage)
2. [What Is an API?](#what-is-an-api)
3. [What Is a REST API?](#what-is-a-rest-api)
4. [Client-Server Architecture](#client-server-architecture)
5. [HTTP Basics](#http-basics)
6. [HTTP Methods](#http-methods)
7. [HTTP Status Codes](#http-status-codes)
8. [Request and Response Structure](#request-and-response-structure)
9. [JSON Format](#json-format)
10. [REST Resources](#rest-resources)
11. [Endpoint Naming Conventions](#endpoint-naming-conventions)
12. [CRUD Operations](#crud-operations)
13. [Sample Data Used in the Examples](#sample-data-used-in-the-examples)
14. [Create Resource with POST](#create-resource-with-post)
15. [Read Resources with GET](#read-resources-with-get)
16. [Update Resource with PUT](#update-resource-with-put)
17. [Update Resource with PATCH](#update-resource-with-patch)
18. [Delete Resource with DELETE](#delete-resource-with-delete)
19. [Path Parameters](#path-parameters)
20. [Query Parameters](#query-parameters)
21. [Request Body](#request-body)
22. [Headers](#headers)
23. [Authentication vs Authorization](#authentication-vs-authorization)
24. [JWT Authentication](#jwt-authentication)
25. [Pagination](#pagination)
26. [Filtering](#filtering)
27. [Sorting](#sorting)
28. [Searching](#searching)
29. [Validation](#validation)
30. [Error Handling](#error-handling)
31. [Idempotency](#idempotency)
32. [API Versioning](#api-versioning)
33. [OpenAPI and Swagger](#openapi-and-swagger)
34. [Testing REST APIs](#testing-rest-apis)
35. [REST API Mini Project](#rest-api-mini-project)
36. [Common API Design Mistakes](#common-api-design-mistakes)
37. [Common Technical Interview Questions](#common-technical-interview-questions)
38. [Study Checklist](#study-checklist)
39. [Recommended Resources](#recommended-resources)

---

## What Is an API?

An **API** means **Application Programming Interface**.

It is a way for two software systems to communicate with each other.

For example:

- A frontend application can request user data from a backend.
- A mobile app can send login credentials to a server.
- A backend service can communicate with a payment provider.
- A chatbot can request information from a document service.

### Simple example

A frontend sends a request:

```http
GET /users/101
```

The backend responds:

```json
{
  "id": 101,
  "name": "Ana Torres",
  "email": "ana.torres@example.com"
}
```

### Result explanation

The API acts as a bridge between the client and the server. The client asks for a resource, and the server returns a structured response.

---

## What Is a REST API?

**REST** means **Representational State Transfer**.

A REST API is an architectural style for designing APIs around resources.

A resource can be:

- A user
- A product
- A task
- A project
- An order
- A document
- A comment

### REST example

| Request | Meaning |
|---|---|
| `GET /users` | Get the list of users |
| `GET /users/101` | Get user 101 |
| `POST /users` | Create a new user |
| `PUT /users/101` | Replace user 101 |
| `PATCH /users/101` | Partially update user 101 |
| `DELETE /users/101` | Delete user 101 |

### Expected behavior

A REST API uses the same URL resource with different HTTP methods to represent different actions.

---

## Client-Server Architecture

REST APIs usually follow a **client-server model**.

```mermaid
graph TD;
    Client[Client Application] -->|HTTP Request| API[REST API Server];
    API -->|HTTP Response| Client;
    API --> Database[(Database)];
```

### Example

A React frontend sends:

```http
GET /tasks
```

The backend API queries the database and returns:

```json
[
  {
    "id": 1,
    "title": "Study REST APIs",
    "completed": false
  },
  {
    "id": 2,
    "title": "Create README",
    "completed": true
  }
]
```

### Explanation

The frontend does not access the database directly. It communicates with the backend through HTTP requests.

---

## HTTP Basics

HTTP means **Hypertext Transfer Protocol**.

It is the protocol commonly used by browsers, mobile apps, and web clients to communicate with servers.

An HTTP interaction has two main parts:

1. Request
2. Response

### HTTP request example

```http
GET /api/users/101 HTTP/1.1
Host: example.com
Accept: application/json
```

### HTTP response example

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 101,
  "name": "Ana Torres"
}
```

### Expected result

| Part | Value |
|---|---|
| Method | `GET` |
| Resource | `/api/users/101` |
| Status code | `200 OK` |
| Response format | JSON |
| Returned user | Ana Torres |

---

## HTTP Methods

HTTP methods describe the action that the client wants to perform.

| Method | Purpose | Common Use |
|---|---|---|
| `GET` | Read data | Get users, tasks, products |
| `POST` | Create data | Create a user or task |
| `PUT` | Replace data | Replace an entire resource |
| `PATCH` | Partially update data | Update one or more fields |
| `DELETE` | Delete data | Delete a resource |

### Example resource

```text
/tasks
```

### Method examples

| Method | Endpoint | Meaning | Expected effect |
|---|---|---|---|
| `GET` | `/tasks` | Get all tasks | Data is returned, nothing changes |
| `GET` | `/tasks/1` | Get task 1 | One task is returned |
| `POST` | `/tasks` | Create a new task | A new row/resource is created |
| `PUT` | `/tasks/1` | Replace task 1 | The whole task is replaced |
| `PATCH` | `/tasks/1` | Update part of task 1 | Only selected fields change |
| `DELETE` | `/tasks/1` | Delete task 1 | The task is removed |

---

## HTTP Status Codes

HTTP status codes indicate the result of a request.

### Common success codes

| Code | Meaning | Example |
|---:|---|---|
| `200` | OK | Successful `GET`, `PUT`, or `PATCH` |
| `201` | Created | Successful `POST` |
| `204` | No Content | Successful `DELETE` with no response body |

### Common client error codes

| Code | Meaning | Example |
|---:|---|---|
| `400` | Bad Request | Invalid request syntax |
| `401` | Unauthorized | Missing or invalid authentication |
| `403` | Forbidden | Authenticated but not allowed |
| `404` | Not Found | Resource does not exist |
| `409` | Conflict | Duplicate email or conflicting state |
| `422` | Unprocessable Entity | Validation error |

### Common server error codes

| Code | Meaning | Example |
|---:|---|---|
| `500` | Internal Server Error | Unexpected backend error |
| `503` | Service Unavailable | Server temporarily unavailable |

### Example

Request:

```http
GET /users/999
```

Response:

```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
  "error": "User not found"
}
```

### Expected result

The server could not find user `999`, so it returns `404 Not Found`.

---

## Request and Response Structure

### Request structure

A request can contain:

- Method
- URL
- Headers
- Query parameters
- Path parameters
- Body

Example:

```http
POST /api/tasks HTTP/1.1
Host: example.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOiJIUzI1...

{
  "title": "Study REST APIs",
  "description": "Learn methods, status codes, and JSON",
  "completed": false
}
```

### Response structure

A response can contain:

- Status code
- Headers
- Body

Example:

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": 1,
  "title": "Study REST APIs",
  "description": "Learn methods, status codes, and JSON",
  "completed": false
}
```

### Expected result

| Request field | Value |
|---|---|
| Method | `POST` |
| Endpoint | `/api/tasks` |
| Body format | JSON |
| Expected status | `201 Created` |
| Expected effect | A new task is created |

---

## JSON Format

JSON means **JavaScript Object Notation**.

It is the most common data format used in REST APIs.

### JSON object

```json
{
  "id": 101,
  "name": "Ana Torres",
  "active": true
}
```

### JSON array

```json
[
  {
    "id": 101,
    "name": "Ana Torres"
  },
  {
    "id": 102,
    "name": "Luis Perez"
  }
]
```

### Important JSON rules

- Keys must be inside double quotes.
- Strings must be inside double quotes.
- Values can be strings, numbers, booleans, arrays, objects, or null.
- JSON does not support comments.

---

## REST Resources

A resource represents an entity managed by the API.

Examples:

| Resource | Endpoint |
|---|---|
| Users | `/users` |
| Tasks | `/tasks` |
| Projects | `/projects` |
| Products | `/products` |
| Orders | `/orders` |
| Documents | `/documents` |

### Good resource naming

Use plural nouns:

```http
/users
/tasks
/projects
```

Avoid verbs in endpoint names:

```http
/getUsers
/createTask
/deleteUser
```

Instead, use HTTP methods:

```http
GET /users
POST /tasks
DELETE /users/101
```

---

## Endpoint Naming Conventions

### Recommended style

Use lowercase nouns and hyphens when needed.

```http
/users
/user-profiles
/project-tasks
```

### Nested resources

Use nested routes when one resource belongs to another.

```http
GET /projects/10/tasks
```

Meaning:

> Get all tasks that belong to project 10.

```http
POST /projects/10/tasks
```

Meaning:

> Create a task inside project 10.

### Avoid deeply nested endpoints

Avoid:

```http
/users/1/projects/2/tasks/3/comments/4/replies/5
```

Prefer simpler routes:

```http
/tasks/3/comments
/comments/4/replies
```

---

## CRUD Operations

CRUD means:

- **Create**
- **Read**
- **Update**
- **Delete**

A REST API commonly maps CRUD to HTTP methods.

| CRUD Operation | HTTP Method | Endpoint Example |
|---|---|---|
| Create | `POST` | `/tasks` |
| Read all | `GET` | `/tasks` |
| Read one | `GET` | `/tasks/1` |
| Update full | `PUT` | `/tasks/1` |
| Update partial | `PATCH` | `/tasks/1` |
| Delete | `DELETE` | `/tasks/1` |

---

## Sample Data Used in the Examples

The following sample data is used in the examples.

### `tasks`

| id | title | description | completed | priority |
|---:|---|---|---|---|
| 1 | Study REST APIs | Learn HTTP, JSON, and endpoints | false | high |
| 2 | Create README | Document API concepts | true | medium |
| 3 | Practice Postman | Test API requests | false | low |

### `users`

| id | name | email | role |
|---:|---|---|---|
| 101 | Ana Torres | ana.torres@example.com | admin |
| 102 | Luis Perez | luis.perez@example.com | user |
| 103 | Marta Gomez | marta.gomez@example.com | user |

---

## Create Resource with POST

### Request

```http
POST /tasks
Content-Type: application/json

{
  "title": "Learn FastAPI",
  "description": "Build REST APIs with Python",
  "completed": false,
  "priority": "high"
}
```

### Expected response

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": 4,
  "title": "Learn FastAPI",
  "description": "Build REST APIs with Python",
  "completed": false,
  "priority": "high"
}
```

### Result in table

Before:

| id | title | completed | priority |
|---:|---|---|---|
| 1 | Study REST APIs | false | high |
| 2 | Create README | true | medium |
| 3 | Practice Postman | false | low |

After:

| id | title | completed | priority |
|---:|---|---|---|
| 1 | Study REST APIs | false | high |
| 2 | Create README | true | medium |
| 3 | Practice Postman | false | low |
| 4 | Learn FastAPI | false | high |

### Explanation

`POST /tasks` creates a new task. The server generates the new `id`.

---

## Read Resources with GET

### Get all tasks

Request:

```http
GET /tasks
Accept: application/json
```

Expected response:

```json
[
  {
    "id": 1,
    "title": "Study REST APIs",
    "description": "Learn HTTP, JSON, and endpoints",
    "completed": false,
    "priority": "high"
  },
  {
    "id": 2,
    "title": "Create README",
    "description": "Document API concepts",
    "completed": true,
    "priority": "medium"
  },
  {
    "id": 3,
    "title": "Practice Postman",
    "description": "Test API requests",
    "completed": false,
    "priority": "low"
  }
]
```

### Get one task

Request:

```http
GET /tasks/1
Accept: application/json
```

Expected response:

```json
{
  "id": 1,
  "title": "Study REST APIs",
  "description": "Learn HTTP, JSON, and endpoints",
  "completed": false,
  "priority": "high"
}
```

### Resource not found

Request:

```http
GET /tasks/999
```

Expected response:

```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
  "error": "Task not found"
}
```

---

## Update Resource with PUT

`PUT` replaces the full resource.

### Request

```http
PUT /tasks/1
Content-Type: application/json

{
  "title": "Study REST APIs deeply",
  "description": "Review HTTP, JSON, endpoints, and authentication",
  "completed": true,
  "priority": "high"
}
```

### Expected response

```json
{
  "id": 1,
  "title": "Study REST APIs deeply",
  "description": "Review HTTP, JSON, endpoints, and authentication",
  "completed": true,
  "priority": "high"
}
```

### Result in table

Before:

| id | title | completed | priority |
|---:|---|---|---|
| 1 | Study REST APIs | false | high |

After:

| id | title | completed | priority |
|---:|---|---|---|
| 1 | Study REST APIs deeply | true | high |

### Explanation

The whole task is replaced with the new representation sent in the request body.

---

## Update Resource with PATCH

`PATCH` updates only specific fields.

### Request

```http
PATCH /tasks/1
Content-Type: application/json

{
  "completed": true
}
```

### Expected response

```json
{
  "id": 1,
  "title": "Study REST APIs",
  "description": "Learn HTTP, JSON, and endpoints",
  "completed": true,
  "priority": "high"
}
```

### Result in table

Before:

| id | title | completed | priority |
|---:|---|---|---|
| 1 | Study REST APIs | false | high |

After:

| id | title | completed | priority |
|---:|---|---|---|
| 1 | Study REST APIs | true | high |

### Explanation

Only the `completed` field changes. The other fields remain the same.

---

## Delete Resource with DELETE

### Request

```http
DELETE /tasks/3
```

### Expected response

```http
HTTP/1.1 204 No Content
```

### Result in table

Before:

| id | title | completed |
|---:|---|---|
| 1 | Study REST APIs | false |
| 2 | Create README | true |
| 3 | Practice Postman | false |

After:

| id | title | completed |
|---:|---|---|
| 1 | Study REST APIs | false |
| 2 | Create README | true |

### Explanation

Task `3` is removed. A `204 No Content` response means the request succeeded but there is no body to return.

---

## Path Parameters

A path parameter is part of the URL.

Example:

```http
GET /tasks/1
```

Here, `1` is a path parameter.

### Backend interpretation

```text
task_id = 1
```

### Expected response

```json
{
  "id": 1,
  "title": "Study REST APIs",
  "completed": false
}
```

### Common use

Path parameters are used to identify a specific resource.

Examples:

```http
GET /users/101
GET /projects/5
DELETE /tasks/3
```

---

## Query Parameters

Query parameters are added after `?` in the URL.

Example:

```http
GET /tasks?completed=false&priority=high
```

### Backend interpretation

```text
completed = false
priority = high
```

### Expected response

```json
[
  {
    "id": 1,
    "title": "Study REST APIs",
    "completed": false,
    "priority": "high"
  }
]
```

### Explanation

Query parameters are commonly used for filtering, sorting, pagination, and searching.

---

## Request Body

The request body contains data sent by the client.

It is commonly used with:

- `POST`
- `PUT`
- `PATCH`

### Example

```http
POST /users
Content-Type: application/json

{
  "name": "Carlos Diaz",
  "email": "carlos.diaz@example.com",
  "role": "user"
}
```

### Expected response

```json
{
  "id": 104,
  "name": "Carlos Diaz",
  "email": "carlos.diaz@example.com",
  "role": "user"
}
```

### Result in table

Before:

| id | name | email | role |
|---:|---|---|---|
| 101 | Ana Torres | ana.torres@example.com | admin |
| 102 | Luis Perez | luis.perez@example.com | user |
| 103 | Marta Gomez | marta.gomez@example.com | user |

After:

| id | name | email | role |
|---:|---|---|---|
| 101 | Ana Torres | ana.torres@example.com | admin |
| 102 | Luis Perez | luis.perez@example.com | user |
| 103 | Marta Gomez | marta.gomez@example.com | user |
| 104 | Carlos Diaz | carlos.diaz@example.com | user |

---

## Headers

Headers provide metadata about the request or response.

### Common request headers

| Header | Purpose |
|---|---|
| `Content-Type` | Tells the server the format of the request body |
| `Accept` | Tells the server what response format is expected |
| `Authorization` | Sends credentials or tokens |
| `User-Agent` | Identifies the client |
| `Cache-Control` | Controls caching behavior |

### Example

```http
GET /users/me
Accept: application/json
Authorization: Bearer eyJhbGciOiJIUzI1...
```

### Explanation

The `Authorization` header sends the access token used to identify the user.

---

## Authentication vs Authorization

### Authentication

Authentication answers:

> Who are you?

Example:

```http
POST /auth/login
```

Request body:

```json
{
  "email": "ana.torres@example.com",
  "password": "Secret123"
}
```

Expected response:

```json
{
  "access_token": "eyJhbGciOiJIUzI1...",
  "token_type": "bearer"
}
```

### Authorization

Authorization answers:

> What are you allowed to do?

Example:

```http
DELETE /users/102
Authorization: Bearer eyJhbGciOiJIUzI1...
```

Possible response if the user is not an admin:

```http
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
  "error": "You do not have permission to delete users"
}
```

---

## JWT Authentication

JWT means **JSON Web Token**.

It is commonly used to authenticate API requests.

### Login request

```http
POST /auth/login
Content-Type: application/json

{
  "email": "ana.torres@example.com",
  "password": "Secret123"
}
```

### Login response

```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "bearer"
}
```

### Authenticated request

```http
GET /users/me
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Expected response

```json
{
  "id": 101,
  "name": "Ana Torres",
  "email": "ana.torres@example.com",
  "role": "admin"
}
```

### Missing token response

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json

{
  "error": "Authentication credentials were not provided"
}
```

---

## Pagination

Pagination limits how many records are returned in one response.

### Request

```http
GET /tasks?page=1&limit=2
```

### Expected response

```json
{
  "page": 1,
  "limit": 2,
  "total": 3,
  "items": [
    {
      "id": 1,
      "title": "Study REST APIs"
    },
    {
      "id": 2,
      "title": "Create README"
    }
  ]
}
```

### Page 2 request

```http
GET /tasks?page=2&limit=2
```

Expected response:

```json
{
  "page": 2,
  "limit": 2,
  "total": 3,
  "items": [
    {
      "id": 3,
      "title": "Practice Postman"
    }
  ]
}
```

### Explanation

The API returns only part of the collection. This improves performance and avoids sending too much data at once.

---

## Filtering

Filtering returns only records that match a condition.

### Request

```http
GET /tasks?completed=false
```

### Expected response

```json
[
  {
    "id": 1,
    "title": "Study REST APIs",
    "completed": false,
    "priority": "high"
  },
  {
    "id": 3,
    "title": "Practice Postman",
    "completed": false,
    "priority": "low"
  }
]
```

### Multiple filters

```http
GET /tasks?completed=false&priority=high
```

Expected response:

```json
[
  {
    "id": 1,
    "title": "Study REST APIs",
    "completed": false,
    "priority": "high"
  }
]
```

---

## Sorting

Sorting changes the order of the results.

### Request

```http
GET /tasks?sort=priority
```

### Alternative request

```http
GET /tasks?sort=created_at&order=desc
```

### Example response

```json
[
  {
    "id": 3,
    "title": "Practice Postman",
    "priority": "low"
  },
  {
    "id": 2,
    "title": "Create README",
    "priority": "medium"
  },
  {
    "id": 1,
    "title": "Study REST APIs",
    "priority": "high"
  }
]
```

### Explanation

Sorting allows clients to control how results are ordered.

---

## Searching

Searching returns records that match a text query.

### Request

```http
GET /tasks?search=REST
```

### Expected response

```json
[
  {
    "id": 1,
    "title": "Study REST APIs",
    "description": "Learn HTTP, JSON, and endpoints"
  }
]
```

### Explanation

The API searches for tasks that contain the word `REST`.

---

## Validation

Validation ensures that the request data is correct before saving or processing it.

### Invalid request

```http
POST /tasks
Content-Type: application/json

{
  "title": "",
  "completed": "no"
}
```

### Expected response

```http
HTTP/1.1 422 Unprocessable Entity
Content-Type: application/json

{
  "errors": [
    {
      "field": "title",
      "message": "Title is required"
    },
    {
      "field": "completed",
      "message": "Completed must be a boolean value"
    }
  ]
}
```

### Explanation

The server rejects invalid data and explains which fields are incorrect.

---

## Error Handling

A good API should return consistent error responses.

### Recommended error format

```json
{
  "error": {
    "code": "TASK_NOT_FOUND",
    "message": "Task not found",
    "details": {
      "task_id": 999
    }
  }
}
```

### Example

Request:

```http
GET /tasks/999
```

Response:

```http
HTTP/1.1 404 Not Found
Content-Type: application/json

{
  "error": {
    "code": "TASK_NOT_FOUND",
    "message": "Task not found",
    "details": {
      "task_id": 999
    }
  }
}
```

### Explanation

The client can use the `code` field to handle errors programmatically.

---

## Idempotency

Idempotency means that making the same request multiple times produces the same final result.

| Method | Idempotent? | Explanation |
|---|---|---|
| `GET` | Yes | Reading data does not change the server state |
| `PUT` | Yes | Replacing the same resource with the same data gives the same final state |
| `PATCH` | Usually depends | It depends on the operation |
| `DELETE` | Yes | Deleting the same resource multiple times leaves it deleted |
| `POST` | Usually no | Multiple requests can create multiple resources |

### Example of idempotent request

```http
PUT /tasks/1
Content-Type: application/json

{
  "title": "Study REST APIs",
  "completed": true
}
```

Sending this request once or many times leaves the task in the same final state.

### Example of non-idempotent request

```http
POST /tasks
Content-Type: application/json

{
  "title": "New task"
}
```

Sending this request multiple times may create multiple tasks.

---

## API Versioning

API versioning allows changes without breaking existing clients.

### URL versioning

```http
/api/v1/tasks
/api/v2/tasks
```

### Header versioning

```http
GET /tasks
Accept: application/vnd.example.v1+json
```

### Recommended simple approach

For beginner and portfolio projects, URL versioning is usually easier:

```http
/api/v1/tasks
```

---

## OpenAPI and Swagger

**OpenAPI** is a standard format for documenting APIs.

**Swagger UI** is a visual interface that allows users to explore and test API endpoints.

### Example OpenAPI-style endpoint description

```yaml
paths:
  /tasks:
    get:
      summary: Get all tasks
      responses:
        "200":
          description: List of tasks
    post:
      summary: Create a task
      responses:
        "201":
          description: Task created
```

### Benefits

- Documents endpoints.
- Shows request and response schemas.
- Helps frontend developers understand the API.
- Allows testing endpoints from the browser.
- Improves collaboration.

---

## Testing REST APIs

REST APIs can be tested manually or automatically.

### Manual testing tools

- Browser
- Postman
- Insomnia
- cURL
- Swagger UI

### cURL examples

#### GET

```bash
curl -X GET http://localhost:8000/tasks
```

#### POST

```bash
curl -X POST http://localhost:8000/tasks \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Learn FastAPI",
    "description": "Build APIs with Python",
    "completed": false,
    "priority": "high"
  }'
```

#### Authenticated request

```bash
curl -X GET http://localhost:8000/users/me \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"
```

### Automated testing idea

Example test case:

```text
Given a valid task payload
When the client sends POST /tasks
Then the API should return 201 Created
And the response should include the generated task ID
```

---

## REST API Mini Project

Build a simple **Task Manager REST API**.

### Main resources

- Users
- Tasks
- Projects

### Suggested endpoints

#### Auth

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/auth/register` | Register a new user |
| `POST` | `/auth/login` | Authenticate user |
| `GET` | `/users/me` | Get current user profile |

#### Tasks

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/tasks` | Get all tasks |
| `GET` | `/tasks/{task_id}` | Get one task |
| `POST` | `/tasks` | Create task |
| `PUT` | `/tasks/{task_id}` | Replace task |
| `PATCH` | `/tasks/{task_id}` | Partially update task |
| `DELETE` | `/tasks/{task_id}` | Delete task |

#### Projects

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/projects` | Get all projects |
| `GET` | `/projects/{project_id}` | Get one project |
| `POST` | `/projects` | Create project |
| `GET` | `/projects/{project_id}/tasks` | Get tasks for a project |

### Suggested database tables

#### `users`

| Column | Type |
|---|---|
| `id` | integer |
| `name` | string |
| `email` | string |
| `password_hash` | string |
| `role` | string |

#### `tasks`

| Column | Type |
|---|---|
| `id` | integer |
| `title` | string |
| `description` | string |
| `completed` | boolean |
| `priority` | string |
| `user_id` | integer |
| `project_id` | integer |

#### `projects`

| Column | Type |
|---|---|
| `id` | integer |
| `name` | string |
| `description` | string |
| `owner_id` | integer |

### Expected features

- CRUD for tasks.
- CRUD for projects.
- User registration and login.
- JWT authentication.
- Input validation.
- Error handling.
- Pagination.
- Filtering.
- Swagger/OpenAPI documentation.
- Basic automated tests.

---

## Common API Design Mistakes

| Mistake | Better Approach |
|---|---|
| Using verbs in endpoints like `/getUsers` | Use `GET /users` |
| Returning `200 OK` for every response | Use proper status codes |
| Not validating request data | Validate body, params, and headers |
| Returning inconsistent errors | Use a standard error format |
| Exposing sensitive data | Never return passwords or secrets |
| Ignoring pagination | Paginate large collections |
| Not documenting endpoints | Use OpenAPI / Swagger |
| Using one endpoint for everything | Design resource-based endpoints |
| Not protecting private routes | Use authentication and authorization |
| Not testing the API | Add manual and automated tests |

---

## Common Technical Interview Questions

### Basic Questions

1. What is an API?
2. What is REST?
3. What is HTTP?
4. What is JSON?
5. What is an endpoint?
6. What is a resource in REST?
7. What is the difference between request and response?
8. What is the difference between frontend and backend in API communication?

### HTTP Questions

1. What is the difference between `GET` and `POST`?
2. What is the difference between `PUT` and `PATCH`?
3. What is the purpose of `DELETE`?
4. What does status code `200` mean?
5. What does status code `201` mean?
6. What does status code `204` mean?
7. What does status code `400` mean?
8. What does status code `401` mean?
9. What does status code `403` mean?
10. What does status code `404` mean?
11. What does status code `500` mean?

### REST Design Questions

1. Why should REST endpoints use nouns instead of verbs?
2. What is a path parameter?
3. What is a query parameter?
4. How would you design endpoints for a task manager?
5. How would you design endpoints for users and roles?
6. How do you handle pagination?
7. How do you handle filtering and sorting?
8. What is API versioning?
9. What is idempotency?
10. Which HTTP methods are idempotent?

### Security Questions

1. What is authentication?
2. What is authorization?
3. What is JWT?
4. Where is a JWT usually sent in a request?
5. What is the `Authorization` header?
6. What is the difference between `401` and `403`?
7. Why should passwords never be returned in API responses?
8. Why should APIs validate all input data?

### Practical Questions

1. Design a REST API for a blog system.
2. Design a REST API for a banking app.
3. Design a REST API for a project management app.
4. Create a JSON request body for user registration.
5. Create a response for a validation error.
6. Create a response for a resource not found error.
7. Explain how a frontend consumes a REST API.
8. Explain how you would test a REST API.

---

## Study Checklist

### API Fundamentals

- [ ] Understand what an API is.
- [ ] Understand what REST means.
- [ ] Understand client-server communication.
- [ ] Understand HTTP requests and responses.
- [ ] Understand JSON format.

### HTTP

- [ ] Use `GET`.
- [ ] Use `POST`.
- [ ] Use `PUT`.
- [ ] Use `PATCH`.
- [ ] Use `DELETE`.
- [ ] Understand common HTTP status codes.

### REST Design

- [ ] Identify resources.
- [ ] Design plural noun endpoints.
- [ ] Use path parameters.
- [ ] Use query parameters.
- [ ] Use request bodies.
- [ ] Use headers.
- [ ] Design CRUD endpoints.

### API Features

- [ ] Add pagination.
- [ ] Add filtering.
- [ ] Add sorting.
- [ ] Add searching.
- [ ] Add validation.
- [ ] Add error handling.
- [ ] Add API versioning.

### Security Basics

- [ ] Understand authentication.
- [ ] Understand authorization.
- [ ] Understand JWT.
- [ ] Use the `Authorization` header.
- [ ] Protect private endpoints.
- [ ] Avoid exposing sensitive data.

### Documentation and Testing

- [ ] Understand OpenAPI.
- [ ] Use Swagger UI.
- [ ] Test with Postman.
- [ ] Test with cURL.
- [ ] Write basic automated test cases.

### Portfolio Project

- [ ] Build a Task Manager API.
- [ ] Add users.
- [ ] Add authentication.
- [ ] Add CRUD operations.
- [ ] Add pagination and filtering.
- [ ] Add validation and error handling.
- [ ] Document the API.
- [ ] Test the API.

---

## Recommended Resources

- MDN HTTP Overview: https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview
- MDN HTTP Methods: https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods
- MDN HTTP Status Codes: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status
- REST API Tutorial: https://restfulapi.net/
- OpenAPI Specification: https://spec.openapis.org/oas/latest.html
- Swagger Documentation: https://swagger.io/docs/
- Postman Learning Center: https://learning.postman.com/docs/introduction/overview/
- JSON Introduction: https://www.json.org/json-en.html

---

## Notes

This README is part of the `developer-knowledge-base` repository.

Recommended file path:

```text
backend/api-rest/README.md
```
