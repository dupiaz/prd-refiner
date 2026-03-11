---
name: api-design
description: REST API best practices, OpenAPI/Swagger spec, versioning, error handling, pagination. Dùng khi thiết kế hoặc review API cho web app.
---

# API Design — REST Best Practices & OpenAPI

Skill hướng dẫn thiết kế REST API theo best practices, bao gồm resource naming, error handling, versioning, và OpenAPI spec authoring.

**Source**: Custom skill — tham khảo `addyosmani/agent-skills` (`api-and-interface-design`).

## Khi Nào Dùng

- Thiết kế API cho web app mới
- Review API endpoints hiện có
- Viết OpenAPI/Swagger spec
- Cần chuẩn hóa error handling
- Thiết kế pagination, filtering, sorting
- Implement API versioning

## Nguyên Tắc Thiết Kế

### 1. Resource Naming

```
# ✅ ĐÚNG — danh từ số nhiều, lowercase, kebab-case
GET    /api/v1/users
GET    /api/v1/users/{id}
POST   /api/v1/users
PUT    /api/v1/users/{id}
PATCH  /api/v1/users/{id}
DELETE /api/v1/users/{id}

# ✅ Nested resources
GET    /api/v1/users/{userId}/orders
GET    /api/v1/users/{userId}/orders/{orderId}

# ✅ Tên composed dùng kebab-case
GET    /api/v1/order-items
GET    /api/v1/user-profiles

# ❌ SAI
GET    /api/v1/getUsers          ← động từ
GET    /api/v1/user              ← số ít
GET    /api/v1/User_Profiles     ← PascalCase + underscore
POST   /api/v1/users/create      ← thừa động từ
```

### 2. HTTP Methods

| Method | Mục đích | Idempotent | Request Body | Response |
|--------|---------|------------|-------------|----------|
| `GET` | Đọc resource(s) | ✅ | Không | Resource(s) |
| `POST` | Tạo resource mới | ❌ | Có | Resource + 201 |
| `PUT` | Thay thế toàn bộ | ✅ | Có | Resource + 200 |
| `PATCH` | Cập nhật một phần | ❌ | Có | Resource + 200 |
| `DELETE` | Xóa resource | ✅ | Không | 204 No Content |

### 3. HTTP Status Codes

```
# Success
200 OK              — GET, PUT, PATCH thành công
201 Created         — POST tạo thành công (kèm Location header)
204 No Content      — DELETE thành công

# Client Error
400 Bad Request     — Request body/params sai format
401 Unauthorized    — Chưa xác thực (missing/invalid token)
403 Forbidden       — Đã xác thực nhưng không có quyền
404 Not Found       — Resource không tồn tại
409 Conflict        — Xung đột (duplicate email, version conflict)
422 Unprocessable   — Validation failed (dữ liệu đúng format nhưng logic sai)
429 Too Many Reqs   — Rate limit exceeded

# Server Error
500 Internal Error  — Lỗi server không xác định
502 Bad Gateway     — Upstream service lỗi
503 Unavailable     — Service tạm ngừng (maintenance)
```

### 4. Error Response Format (RFC 7807)

```json
{
  "type": "https://api.example.com/errors/validation-error",
  "title": "Validation Error",
  "status": 422,
  "detail": "Email đã được sử dụng bởi tài khoản khác.",
  "instance": "/api/v1/users",
  "errors": [
    {
      "field": "email",
      "code": "DUPLICATE",
      "message": "Email 'user@test.com' đã tồn tại"
    }
  ],
  "traceId": "abc-123-def"
}
```

**Quy tắc Error Response:**
- LUÔN trả JSON, không HTML error pages
- Bao gồm `traceId` để debug
- `errors[]` array cho validation errors
- KHÔNG expose stack trace, database errors ở production
- Log chi tiết ở server, trả message thân thiện cho client

### 5. Pagination

```json
// Request
GET /api/v1/products?page=2&limit=20&sort=-createdAt

// Response
{
  "data": [...],
  "meta": {
    "page": 2,
    "limit": 20,
    "totalItems": 156,
    "totalPages": 8,
    "hasNextPage": true,
    "hasPrevPage": true
  },
  "links": {
    "self": "/api/v1/products?page=2&limit=20",
    "first": "/api/v1/products?page=1&limit=20",
    "prev": "/api/v1/products?page=1&limit=20",
    "next": "/api/v1/products?page=3&limit=20",
    "last": "/api/v1/products?page=8&limit=20"
  }
}
```

**Pagination patterns:**
- **Offset-based** (`page` + `limit`): dễ implement, phù hợp admin panels
- **Cursor-based** (`cursor` + `limit`): performant hơn, phù hợp feeds/timelines
- Default `limit`: 20, max `limit`: 100

### 6. Filtering & Sorting

```
# Filtering
GET /api/v1/products?status=active&category=electronics&price_min=100&price_max=500

# Sorting (prefix - cho descending)
GET /api/v1/products?sort=-price,name

# Search
GET /api/v1/products?q=iphone&fields=name,description

# Field selection (sparse fieldsets)
GET /api/v1/products?fields=id,name,price,thumbnail
```

### 7. API Versioning

```
# ✅ URL Path versioning (đơn giản nhất, khuyến nghị)
GET /api/v1/users
GET /api/v2/users

# Alternatives (ít phổ biến hơn)
# Header versioning
Accept: application/vnd.api+json;version=2

# Query parameter
GET /api/users?version=2
```

**Quy tắc versioning:**
- Chỉ bump major version khi có BREAKING CHANGES
- Duy trì version cũ ít nhất 6 tháng sau khi deprecate
- Trả `Deprecation` header cho version cũ

## OpenAPI Spec Template

```yaml
openapi: 3.1.0
info:
  title: My App API
  version: 1.0.0
  description: API documentation cho My App
  contact:
    email: dev@myapp.com

servers:
  - url: https://api.myapp.com/v1
    description: Production
  - url: http://localhost:3000/api/v1
    description: Development

paths:
  /users:
    get:
      summary: Danh sách users
      operationId: listUsers
      tags: [Users]
      parameters:
        - name: page
          in: query
          schema: { type: integer, default: 1 }
        - name: limit
          in: query
          schema: { type: integer, default: 20, maximum: 100 }
      responses:
        '200':
          description: Thành công
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserListResponse'

    post:
      summary: Tạo user mới
      operationId: createUser
      tags: [Users]
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserRequest'
      responses:
        '201':
          description: Tạo thành công
        '422':
          description: Validation error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'

components:
  schemas:
    User:
      type: object
      properties:
        id: { type: string, format: uuid }
        email: { type: string, format: email }
        name: { type: string }
        createdAt: { type: string, format: date-time }

    CreateUserRequest:
      type: object
      required: [email, name, password]
      properties:
        email: { type: string, format: email }
        name: { type: string, minLength: 2, maxLength: 100 }
        password: { type: string, minLength: 8 }

    ErrorResponse:
      type: object
      properties:
        type: { type: string }
        title: { type: string }
        status: { type: integer }
        detail: { type: string }
        errors:
          type: array
          items:
            type: object
            properties:
              field: { type: string }
              code: { type: string }
              message: { type: string }

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

security:
  - bearerAuth: []
```

## Authentication Patterns

```
# JWT Bearer Token
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...

# API Key (cho server-to-server)
X-API-Key: sk_live_abc123...

# Cookie-based (cho web apps)
Cookie: session=abc123...
```

**Best Practices:**
- JWT cho mobile & SPA, cookies cho SSR web apps
- API keys cho machine-to-machine
- LUÔN dùng HTTPS
- Token expiry: access 15min, refresh 7 days
- Rate limiting: 100 req/min/user default

## Request Validation

```javascript
// Zod schema validation (Node.js/TypeScript)
import { z } from 'zod';

const CreateUserSchema = z.object({
  email: z.string().email('Email không hợp lệ'),
  name: z.string().min(2).max(100),
  password: z.string().min(8).regex(
    /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/,
    'Mật khẩu cần ít nhất 1 chữ hoa, 1 chữ thường, 1 số'
  ),
});

// Validate trong route handler
app.post('/api/v1/users', async (req, res) => {
  const result = CreateUserSchema.safeParse(req.body);
  if (!result.success) {
    return res.status(422).json({
      type: 'validation-error',
      title: 'Validation Error',
      status: 422,
      errors: result.error.issues.map(i => ({
        field: i.path.join('.'),
        code: i.code,
        message: i.message,
      })),
    });
  }
  // ... create user
});
```

## API Design Checklist

- [ ] Resource names: danh từ số nhiều, lowercase, kebab-case
- [ ] HTTP methods đúng semantics
- [ ] Status codes chính xác
- [ ] Error format theo RFC 7807
- [ ] Pagination cho list endpoints
- [ ] Rate limiting headers
- [ ] CORS configured cho web clients
- [ ] Input validation ở API layer
- [ ] OpenAPI spec cập nhật
- [ ] Authentication/authorization cho mọi endpoint
- [ ] Idempotency keys cho POST/payment endpoints
- [ ] Request/response logging (không log sensitive data)
