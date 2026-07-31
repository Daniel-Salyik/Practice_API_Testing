# Test Cases — Booking : GetBooking


**Endpoint:** `GET /booking/:id`
**Auth required:** No
**File:** `docs/test-cases/Getbooking.md`

> ID convention: `GET_<RESOURCE>_<OPERATION>_<NNN>` — e.g. `GET_001`.
> Priority: High / Medium / Low.
> Each case is its own `##` section so request/response JSON renders as code blocks instead of being squeezed into table cells.


## API_BOOKING_GET_001
**Objective:** Successful retrieval of an existing booking by valid id
**Priority:** High
**Preconditions:** A booking must exist

**Request**

`GET {{base_URL}}/booking/{{bookingId}}`

**Expected**
- Status: `200 OK`
- Response: JSON object matching the booking created in the preconditions

**Assertions**
- Status code = 200
- `firstname`, `lastname`, `totalprice` match values from the original create request

---

## API_BOOKING_GET_002
**Objective:** Retrieval attempt with a nonexistent booking id
**Priority:** High
**Preconditions:** The provided id does not exists

**Request**

`GET {{base_URL}}/booking/999999`

**Expected**
- Status: `404 Not Found`

**Assertions**
- Status code = 404

---

## API_BOOKING_GET_003
**Objective:** Retrieval attempt with a non-numeric booking id
**Priority:** High
**Preconditions:** Booking ids are expected to be numeric

**Request**

`GET {{base_URL}}/booking/xyz`

**Expected**
- Status: `404 Not Found`

**Assertions**
- Status code = 404

---

## API_BOOKING_GET_004
**Objective:** Retrieval attempt with a negative booking id
**Priority:** Medium
**Preconditions:** Booking ids are expected to be positive integers

**Request**

`GET {{base_URL}}/booking/-10`

**Expected**
- Status: `404 Not Found`

**Assertions**
- Status code = 404

---