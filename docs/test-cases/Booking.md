# Test Cases — Booking: Create

**Endpoint:** `POST /booking`
**Auth required:** No
**File:** `docs/test-cases/booking-create.md`

> ID convention: `API_<RESOURCE>_<OPERATION>_<NNN>` — e.g. `API_BOOKING_CREATE_001`.
> Priority: High / Medium / Low.
> Each case is its own `##` section so request/response JSON renders as code blocks instead of being squeezed into table cells.

---

## API_BOOKING_CREATE_001
**Objective:** Successful booking creation with valid data
**Priority:** High
**Preconditions:** None

**Request**

Headers: `Content-Type: application/json`

```json
{
  "firstname": "Sally",
  "lastname": "Brown",
  "totalprice": 150,
  "depositpaid": true,
  "bookingdates": {
    "checkin": "2026-07-01",
    "checkout": "2026-07-10"
  },
  "additionalneeds": "Breakfast"
}
```

**Expected**
- Status: `200 OK`
- Response: JSON with `bookingid` (number) and a `booking` object matching the request

**Assertions**
- Status code = 200
- `bookingid` is present and is a number
- `booking.firstname` === request `firstname`
- `booking.bookingdates.checkin` === request `checkin`

---

## API_BOOKING_CREATE_002
**Objective:** Verify behavior when a required field (`firstname`) is missing
**Priority:** High
**Preconditions:** None

**Request**

Headers: `Content-Type: application/json`

```json
{
  "lastname": "Brown",
  "totalprice": 150,
  "depositpaid": true,
  "bookingdates": {
    "checkin": "2026-07-01",
    "checkout": "2026-07-10"
  }
}
```

**Expected**
- Status: `400 Bad Request`
- Response: error message identifying the missing field

**Actual (observed)**
- Status: `500 Internal Server Error`
- Indicates the API doesn't validate input and crashes on missing required fields instead of rejecting them gracefully

**Result:** FAIL — defect. See bug report `BUG-001`.

---

## API_BOOKING_CREATE_003
**Objective:** Verify boundary handling when `totalprice` is `0`
**Priority:** Medium
**Preconditions:** None

**Request**

Headers: `Content-Type: application/json`

```json
{
  "firstname": "Test",
  "lastname": "Zero",
  "totalprice": 0,
  "depositpaid": false,
  "bookingdates": {
    "checkin": "2026-07-01",
    "checkout": "2026-07-02"
  }
}
```
**Expected:**
- No documented constraint on minimum totalprice; testing whether 0 is treated as valid
**Actual:** 
- Status :` 200 OK`, booking created with totalprice = 0
**Result:** 
- PASS — system accepts 0 as a valid boundary value 
---

## API_BOOKING_CREATE_004
**Objective:** Verify rejection of negative totalprice
**Priority:** Medium

**Request**
```json
{
  "firstname": "Test",
  "lastname": "NegativePrice",
  "totalprice": -50,
  "depositpaid": false,
  "bookingdates": { "checkin": "2026-07-01", "checkout": "2026-07-02" }
}
```

**Expected:** `400 Bad Request` — negative price is invalid regardless of business rules
**Actual:** 
`200 OK`, booking created with totalprice = -50 (negative value persisted as-is)
**Result:** FAIL — defect. See bug report BUG-002.


## API_BOOKING_CREATE_005
**Objective:** Verify behavior when `checkout` date is before `checkin` date
**Priority:** Medium
**Preconditions:** None

**Request**

Headers: `Content-Type: application/json`

```json
{
  "firstname": "Test",
  "lastname": "InvalidDates",
  "totalprice": 100,
  "depositpaid": true,
  "bookingdates": {
    "checkin": "2026-07-10",
    "checkout": "2026-07-01"
  }
}
```

**Expected**
- Status: `400 Bad Request`
- Response: error message indicating checkout cannot precede checkin

**Actual (observed)**
- Status: `200 OK`
- Response body returns the dates exactly as sent — checkin: 2026-07-10, checkout: 2026-07-01 — persisted without validation

**Result:** FAIL — defect. See bug report BUG-003.

## API_BOOKING_CREATE_006
**Objective:** Verify behavior when `bookindates` object is empty
**Priority:** Medium
**Preconditions:** None

**Request**

Headers: `Content-Type: application/json`

```json
{
  "firstname": "Test",
  "lastname": "InvalidDates",
  "totalprice": 100,
  "depositpaid": true,
  "bookingdates": {}
}
```

**Expected**
- Status: `400 Bad Request`
- Response: error message indicating checkout cannot precede checkin

**Assertion**
- Status : `500 Internal Server Error`


## API_BOOKING_CREATE_007
**Objective:** Verify behavior when `totalprice` type is string instead of number
**Priority:** Medium
**Preconditions:** None

**Request**

Headers: `Content-Type: application/json`

```json
{
  "firstname": "Sally",
  "lastname": "Brown",
  "totalprice": "abc",
  "depositpaid": true,
  "bookingdates": {
    "checkin": "2026-07-01",
    "checkout": "2026-07-10"
  },
  "additionalneeds": "Breakfast"
}
```

**Expected**
- Status: `400 Bad Request`
- Response: error message indicating totalprice must be a number

**Actual (observed)**
- Status: `200 OK`
- Response body: `totalprice` is `null`

**Result:** FAIL — defect. See bug report BUG-006.

## API_BOOKING_CREATE_008
**Objective:** Verify behavior when a required field (`firstname`) is an empty string
**Priority:** High
**Preconditions:** None

**Request**

Headers: `Content-Type: application/json`

```json
{
  "firstname": "",
  "lastname": "Brown",
  "totalprice": 150,
  "depositpaid": true,
  "bookingdates": {
    "checkin": "2026-07-01",
    "checkout": "2026-07-10"
  },
  "additionalneeds": "Breakfast"
}
```

**Expected**
- Status: `400 Bad Request`
- Response: error message indicating firstname field cannot be an empty string

**Actual (observed)**
- Status: `200 OK`
- Response body: `firstname` is set to empty string

**Result:** FAIL — defect. See bug report BUG-007.


## API_BOOKING_CREATE_009
**Objective:** Verify behavior when a unknown field in payload
**Priority:** High
**Preconditions:** None

**Request**

Headers: `Content-Type: application/json`

```json
{
  "firstname": "Sally",
  "lastname": "Brown",
  "totalprice": 150,
  "depositpaid": true,
  "bookingdates": {
    "checkin": "2026-07-01",
    "checkout": "2026-07-10"
  },
  "additionalneeds": "Breakfast",
  "unknown" : "bad payload"
}
```

**Expected**
- Status: `200 OK`
- Unrecognized fields should not be included in the response body

**Actual (observed)**
- Status: `200 OK`
- Response body does not include the `unknown` field 

**Result:** PASS — unrecognized fields are correctly ignored rather than stored