# api-design-checklist

# ✅ API Design Checklist

> A practical checklist for designing REST APIs — built from real mistakes I made as a Junior Backend Engineer.
>
> If you're building an API and want it to be **consistent, secure, and maintainable** — go through this list before shipping.

**By [Wongsaphat Nakmuang](https://github.com/Bee34949) · Backend Engineer @ INET**

---

## 🟢 Basic — For Juniors Getting Started

These are the fundamentals. If you skip any of these, your API will cause pain for whoever integrates with it (including future you).

### 📌 Naming & Structure

- [ ] Use **nouns** for endpoints, not verbs
  - ✅ `GET /users` — ❌ `GET /getUsers`
- [ ] Use **plural** resource names consistently
  - ✅ `/users`, `/orders` — ❌ `/user`, `/order`
- [ ] Use **lowercase** and **hyphens** for multi-word paths
  - ✅ `/user-profiles` — ❌ `/userProfiles`, `/user_profiles`
- [ ] Nest resources logically (max 2 levels deep)
  - ✅ `/users/{id}/orders` — ❌ `/users/{id}/orders/{id}/items/{id}/details`

### 📌 HTTP Methods

- [ ] Use the correct HTTP method for each action
  - `GET` → Read (no side effects)
  - `POST` → Create
  - `PUT` → Replace entire resource
  - `PATCH` → Update partial resource
  - `DELETE` → Remove
- [ ] `GET` requests must **never modify data**
- [ ] `DELETE` returns `204 No Content` (not `200` with a body)

### 📌 HTTP Status Codes

- [ ] Return the correct status code — not just `200` for everything
  - `200` OK — success with body
  - `201` Created — resource created (use with `POST`)
  - `204` No Content — success without body
  - `400` Bad Request — client sent invalid data
  - `401` Unauthorized — not authenticated
  - `403` Forbidden — authenticated but no permission
  - `404` Not Found — resource doesn't exist
  - `422` Unprocessable Entity — validation failed
  - `500` Internal Server Error — something broke on your end
- [ ] Never return `200` with `{ "success": false }` in the body

### 📌 Error Response Format (Consistent Every Time)

> This was my biggest lesson — inconsistent error responses break frontend integrations.

- [ ] Every error response uses the **same structure** across all endpoints

```json
{
  "status": "error",
  "code": 400,
  "message": "Validation failed",
  "errors": [
    {
      "field": "email",
      "message": "Email is required"
    }
  ]
}
```

- [ ] Error `message` is human-readable (not "Error 0x00023")
- [ ] Validation errors list **every failing field** — not just the first one
- [ ] Never expose stack traces or internal error details in production
- [ ] Use the same `status` field naming across all responses (`"success"` / `"error"`)

### 📌 Response Structure

- [ ] Wrap all responses in a consistent envelope

```json
{
  "status": "success",
  "data": { ... }
}
```

- [ ] List responses include pagination metadata

```json
{
  "status": "success",
  "data": [...],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 100
  }
}
```

- [ ] Use `camelCase` for JSON field names (consistent throughout)
- [ ] Never return `null` arrays — return `[]` instead

---

## 🔵 Intermediate — For Real-World Production APIs

These are things most tutorials skip. They matter when your API is used by a real team or real users.

### 📌 Authentication & Security

> My second biggest lesson — auth gaps are invisible until something goes wrong.

- [ ] All endpoints that return user data require **authentication**
- [ ] Use **JWT** with expiry — never non-expiring tokens
- [ ] Implement **Refresh Token** rotation (short-lived access token + longer refresh token)
- [ ] Store tokens in `httpOnly` cookies, not `localStorage` (prevents XSS)
- [ ] Validate **all** incoming request data before processing — never trust client input
- [ ] Use HTTPS only — never send tokens over HTTP
- [ ] Add **rate limiting** to auth endpoints (login, register, reset password)
- [ ] Return `401` for unauthenticated, `403` for unauthorized — never mix them
- [ ] Hash passwords with `bcrypt` (min 10 rounds) — never store plain text
- [ ] Sanitize inputs to prevent **SQL Injection** and **NoSQL Injection**

### 📌 Versioning

- [ ] Version your API from **day one** — don't wait until you need to break changes
  - ✅ `/api/v1/users`
- [ ] Never remove or rename fields in an existing version
- [ ] Create a new version (`v2`) for breaking changes — keep `v1` running
- [ ] Document which version is deprecated and when it will be removed

### 📌 Request Validation

- [ ] Validate **type**, **format**, and **required fields** on every request
- [ ] Return `422` with field-level errors for validation failures (not `400`)
- [ ] Set **max length** on all string inputs
- [ ] Validate UUIDs / IDs format before hitting the database
- [ ] Use a validation library (Zod, Joi, Yup) — don't write manual checks

### 📌 Performance Basics

- [ ] Use **pagination** on all list endpoints — never return unbounded arrays
- [ ] Add **indexes** on fields used in `WHERE` / `JOIN` queries
- [ ] Select only needed fields — avoid `SELECT *` in production queries
- [ ] Use **connection pooling** for database connections
- [ ] Set **timeouts** on all external API calls

### 📌 Documentation

- [ ] Every endpoint has a description of what it does
- [ ] Request body and query params are documented with types and examples
- [ ] All possible response codes are listed
- [ ] Authentication requirements are clearly stated
- [ ] Use **OpenAPI / Swagger** to generate interactive docs

---

## 📋 Quick Reference Card

| Category | Most Common Mistake | Fix |
|---|---|---|
| Error Response | Different structure per endpoint | One standard error format |
| Status Codes | Using `200` for everything | Match code to outcome |
| Auth | No token expiry | JWT + refresh token rotation |
| Naming | `/getUser`, `/createPost` | Nouns only: `/users`, `/posts` |
| Validation | Only validate on frontend | Always validate on backend too |
| Versioning | No versioning until too late | `/api/v1/` from day one |
| Lists | Return all records | Always paginate |

---

## 🙋 About This Checklist

This checklist is built from:
- Things I learned in university that clicked during real work
- Mistakes I made (or saw) in my first weeks as a Backend Engineer
- Lessons from mentors who pushed me to think beyond "making it work"

It's a living document — I update it as I learn more.

**PRs and suggestions welcome.** If you've caught a mistake or want to add something, open an issue.

---

## 📬 Connect

- 🐦 Dev.to: [beeworkk](https://dev.to/beeworkk)
- 💼 GitHub: [Bee34949](https://github.com/Bee34949)
- 🔗 LinkedIn: [wongsaphat-nakmuang](https://linkedin.com/in/wongsaphat-nakmuang)

---

<div align="center">
  <sub>Made with ❤️ by a Junior Engineer learning in public 🌱</sub>
</div>
