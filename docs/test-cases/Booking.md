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