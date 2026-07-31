# Test Cases — Booking: Delete

**Endpoint:** `DELETE /booking/{id}`
**Auth required:** Yes 
**File:** `docs/test-cases/delete-booking.md`

> ID convention: `API_<RESOURCE>_<OPERATION>_<NNN>` — e.g. `API_BOOKING_DELETE_001`.
> Priority: High / Medium / Low.
> Each case is its own `##` section so request/response JSON renders as code blocks instead of being squeezed into table cells.

---

## API_BOOKING_DELETE_001
**Objective:** Deletion attempt with no auth token provided
**Priority:** High
**Preconditions:** A booking exists (`{{bookingId}}`)

**Request**

`DELETE {{base_url}}/booking/{{bookingId}}`

Headers: none (no `Cookie`/auth header)

**Expected**
- Status: `403 Forbidden`

**Assertions**
- Status code = 403

---

## API_BOOKING_DELETE_002
**Objective:** Successful deletion of an existing booking with a valid token
**Priority:** High
**Preconditions:** A booking exists (`{{bookingId}}` from CreateBooking), valid token exists (`{{authToken}}` from Auth)

**Request**

`DELETE {{base_url}}/booking/{{bookingId}}`

Headers: `Cookie: token={{authToken}}`

**Expected**
- Status: `201 Created`

**Assertions**
- Status code = 201

## API_BOOKING_DELETE_003
**Objective:** Deletion attempt with an invalid/malformed token
**Priority:** High
**Preconditions:** A booking exists (`{{bookingId}}`)

**Request**

`DELETE {{base_url}}/booking/{{bookingId}}`

Headers: `Cookie: token=invalidtoken123`

**Expected**
- Status: `403 Forbidden`

**Assertions**
- Status code = 403


---

## API_BOOKING_DELETE_004
**Objective:** Deletion attempt on a nonexistent booking id, with a valid token
**Priority:** Medium
**Preconditions:** Valid token exists (`{{authToken}}`); id does not correspond to any existing booking

**Request**

`DELETE {{base_url}}/booking/999999`

Headers: `Cookie: token={{authToken}}`

**Expected**
- Status: 