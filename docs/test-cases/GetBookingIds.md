# Test Cases — Booking : GetBookingIds


**Endpoint:** `GET /booking/:id`
**Auth required:** No
**File:** `docs/test-cases/Getbooking.md`

> ID convention: `API_<RESOURCE>_<OPERATION>_<NNN>` — e.g. `GETIDS_001`.
> Priority: High / Medium / Low.
> Each case is its own `##` section so request/response JSON renders as code blocks instead of being squeezed into table cells.

## API_BOOKING_GETIDS_001
**Objective:** Retrieve list of booking ids with no filters applied
**Priority:** High
**Preconditions:** None

**Request**

`GET {{base_url}}/booking`

**Expected**
- Status: `200 OK`
- Response: JSON array, each element an object containing a numeric `bookingid`
- Array is non-empty

**Assertions**
- Status code = 200
- Response body is an array
- Array length > 0
- Each element has a `bookingid` property
- Each `bookingid` is a number

---

## API_BOOKING_GETIDS_002
**Objective:** Retrieve booking ids filtered by firstname
**Priority:** High
**Preconditions:** A booking has been created in this run with a known firstname

**Request**

`GET {{base_url}}/booking?firstname={{createdFirstname}}`

**Expected**
- Status: `200 OK`
- Response: array containing at least the booking id created in this run (`{{bookingId}}`)

**Assertions**
- Status code = 200
- Response body is an array, not empty
- Array contains an object with `bookingid` matching `{{bookingId}}`


## API_BOOKING_GETIDS_003
**Objective:** Retrieve booking ids filtered by lastname
**Priority:** High
**Preconditions:** A booking has been created in this run with a known lastname

**Request**

`GET {{base_url}}/booking?lastname={{createdLastname}}`

**Expected**
- Status: `200 OK`
- Response: array containing at least the booking id created in this run (`{{bookingId}}`)

**Assertions**
- Status code = 200
- Response body is an array, not empty
- Array contains an object with `bookingid` matching `{{bookingId}}`

---

## API_BOOKING_GETIDS_004
**Objective:** Retrieve booking ids filtered by combined firstname and lastname
**Priority:** Medium
**Preconditions:** A booking has been created in this run with known firstname/lastname

**Request**

`GET {{base_url}}/booking?firstname={{createdFirstname}}&lastname={{createdLastname}}`

**Expected**
- Status: `200 OK`
- Response: array containing the booking id

**Assertions**
- Status code = 200
- Array contains an object with `bookingid` matching `{{bookingId}}`

---

## API_BOOKING_GETIDS_005
**Objective:** Retrieve booking ids filtered by checkin/checkout date range matching the created booking
**Priority:** Medium
**Preconditions:** A booking has been created in this run with known dates

**Request**

`GET {{base_url}}/booking?checkin={{createdCheckin}}&checkout={{createdCheckout}}`

**Expected**
- Status: `200 OK`
- Response: array containing the booking id created in this run

**Assertions**
- Status code = 200
- Array contains an object with `bookingid` matching `{{bookingId}}`

---

## API_BOOKING_GETIDS_006
**Objective:** Retrieve booking ids with a date range that does not match the created booking
**Priority:** Medium
**Preconditions:** A booking has been created in this run with known dates

**Request**

`GET {{base_url}}/booking?checkin=2099-01-01&checkout=2099-01-10`

**Expected**
- Status: `200 OK`
- Response: array does not contain the created booking id with a meaningfull message

**Assertions**
- Status code = 200
- Array does not contain an object with `bookingid` matching `{{bookingId}}`

---


