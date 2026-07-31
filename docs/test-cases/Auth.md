# Test Cases — Auth - CreateToken


**Endpoint:** `POST /auth`
**Auth required:** Yes
**File:** `docs/test-cases/Auth.md`

> ID convention: `API_<RESOURCE>_<OPERATION>_<NNN>` — e.g. `API_AUTH_CREATE_001`.
> Priority: High / Medium / Low.
> Each case is its own `##` section so request/response JSON renders as code blocks instead of being squeezed into table cells.

---

## API_AUTH_CREATE_001
**Objective:** Successful token creation with valid credentials
**Priority:** High
**Preconditions:** None

**Request**

Headers: `Content-Type: application/json`

```json
{
    "username" : "{{valid_username}}",
    "password" : "{{valid_password}}"
}
```

**Expected**
- Status: `200 OK`
- Response: JSON body containing `token` field

**Assertions**
- Status code = 200
- Response body contains `token` field
- `token` is a non-empty string
- Token saved to collection variable `authToken`

---

## API_AUTH_CREATE_002
**Objective:** Failed token creation with invalid username field
**Priority:** High
**Preconditions:** None

**Request**

Headers: `Content-Type: application/json`

```json
{
    "username" : "{{invalid_username}}",
    "password" : "{{valid_password}}"
}
```

**Expected**
- Status: `401 Unauthorized`
- Response body contains `reason` field
- Response body not contains `token` field

**Assertions**
- Status code = 200
- Response body not contains `token` field

---

## API_AUTH_CREATE_003
**Objective:** Failed token creation with invalid password field
**Priority:** High
**Preconditions:** None

**Request**

Headers: `Content-Type: application/json`

```json
{
    "username" : "{{valid_username}}",
    "password" : "{{invalid_password}}"
}
```

**Expected**
- Status: `401 Unauthorized`
- Response body contains `reason` field
- Response body not contains `token` field

**Assertions**
- Status code = 200
- Response body not contains `token` field

---

## API_AUTH_CREATE_004
**Objective:** Failed token creation with empty string credentials
**Priority:** High
**Preconditions:** None

**Request**

Headers: `Content-Type: application/json`

```json
{
    "username" : "",
    "password" : ""
}
```

**Expected**
- Status: `401 Unauthorized`
- Response body contains `reason` field
- Response body not contains `token` field

**Assertions**
- Status code = 200
- Response body contains `reason` field
- Response body not contains `token` field

---