# API Reference

## Base URL

- **Development**: `https://localhost:7028` or `http://localhost:5039`
- **Production**: Configured via `appsettings.json`

## Authentication

Currently, no authentication is implemented. All endpoints are publicly accessible.

## Endpoints

### Employee Management

All employee endpoints are grouped under the `/employees` route group.

#### GET /employees

Returns all employees in the system.

**Request**
```http
GET /employees
Accept: application/json
```

**Response**
```json
[
  {
    "id": 1,
    "firstName": "John",
    "lastName": "Doe"
  },
  {
    "id": 2,
    "firstName": "Jane",
    "lastName": "Doe"
  }
]
```

#### GET /employees/{id}

Returns a specific employee by ID.

**Request**
```http
GET /employees/1
Accept: application/json
```

**Response**
```json
{
  "id": 1,
  "firstName": "John",
  "lastName": "Doe"
}
```

**Route Parameters**

| Parameter | Type | Description | Constraint |
|-----------|------|-------------|------------|
| `id` | integer | Employee ID | Required, must be integer |

#### POST /employees

Creates a new employee.

**Request**
```http
POST /employees
Content-Type: application/json

{
  "firstName": "Alice",
  "lastName": "Smith"
}
```

**Response**
```http
HTTP/1.1 201 Created
Location: /employees/3

{
  "id": 3,
  "firstName": "Alice",
  "lastName": "Smith"
}
```

**Request Body Properties**

| Property | Type | Description | Required |
|----------|------|-------------|----------|
| `firstName` | string | Employee's first name | Yes |
| `lastName` | string | Employee's last name | Yes |

**Status Codes**

| Code | Description |
|------|-------------|
| 200 | Success - Employee data returned |
| 201 | Created - New employee created successfully |
| 404 | Not Found - Employee with specified ID not found |
| 400 | Bad Request - Invalid request body or missing required fields |
| 500 | Internal Server Error |

## Data Models

### Employee

```csharp
public class Employee
{
    public int Id { get; set; }
    public required string FirstName { get; set; }
    public required string LastName { get; set; }
}
```

**Properties:**
- `Id`: Unique identifier for the employee (auto-generated)
- `FirstName`: Employee's first name (required)
- `LastName`: Employee's last name (required)

## Testing

Use the provided `TheEmployeeAPI.http` file for testing endpoints:

```http
@TheEmployeeAPI_HostAddress = http://localhost:5039

### Get all employees
GET {{TheEmployeeAPI_HostAddress}}/employees
Accept: application/json

### Get employee by ID
GET {{TheEmployeeAPI_HostAddress}}/employees/1
Accept: application/json

### Create new employee
POST {{TheEmployeeAPI_HostAddress}}/employees
Content-Type: application/json

{
  "firstName": "Alice",
  "lastName": "Smith"
}
```

## OpenAPI Documentation

In development mode, OpenAPI documentation is available at:
- **Swagger UI**: `https://localhost:7028/swagger` (if configured)
- **OpenAPI JSON**: `https://localhost:7028/openapi/v1.json`

## Error Handling

Currently, the API uses default ASP.NET Core error handling. No custom error responses are implemented.

## Rate Limiting

No rate limiting is currently implemented. All endpoints are accessible without restrictions.

## CORS

CORS is not explicitly configured. The default behavior allows requests from the same origin.
