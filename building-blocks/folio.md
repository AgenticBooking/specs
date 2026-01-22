---
title: Folio
order: 4
---

# Folio

The payment semantics for hospitality bookings.

---

## What is Folio?

**Folio** defines the financial terms of a booking—what's owed, when it's due, and what happens if plans change. Where Stay tracks lifecycle states, Folio tracks money.

Hospitality transactions are agreements governing future-conditional payment execution, not immediate exchanges of value. When you book a hotel, you might pay a deposit now, balance at check-in, get a refund if you cancel, or be charged if you don't show up.

::callout{type="info"}
Folio is a vocabulary, not a payment processor. It defines how to express hospitality payment terms within existing protocols.
::

---

## Why Hospitality is Different

| Aspect | Retail | Hospitality |
|--------|--------|-------------|
| Payment timing | At purchase | Split across dates |
| Fulfilment | Days after payment | Weeks/months after booking |
| Cancellation | Rarely allowed | Core feature with refund decay |
| No-show | N/A | Triggers penalty charge |

```
Retail:      Intent → Cart → Pay → Ship → Done

Hospitality: Intent → Book → Deposit → [weeks] → Balance → Stay
                                  ↓
                            Cancel? Modify? No-show?
```

---

## Core Concepts

::card-group{cols="3"}
  ::card{title="Payment Schedule" to="/reference/folio/spec#2-payment-schedule"}
  Defines when money moves: deposit at booking, balance before arrival.
  ::
  ::card{title="Cancellation Policy" to="/reference/folio/spec#3-cancellation-policy"}
  Tiered refund decay based on days before check-in.
  ::
  ::card{title="No-Show Policy" to="/reference/folio/spec#4-no-show-policy"}
  What happens if the guest doesn't arrive.
  ::
::

::card-group{cols="2"}
  ::card{title="Modification Policy" to="/reference/folio/spec#5-modification-policy"}
  Rules for changing dates, rooms, or guests after booking.
  ::
  ::card{title="Mandate Integration" to="/reference/folio/spec#6-mandate-integration"}
  Cryptographic proof of what the user authorized.
  ::
::

---

## How Agents Use Folio

::steps
### Check Terms Before Booking

Query the venue's Folio policies to understand payment schedule, cancellation terms, and modification rules before presenting options.

### Present Terms Clearly

Ensure the user understands when money will be charged and what happens if plans change before confirming the booking.

### Handle Cancellations

Calculate refund amount based on cancellation tier and days before arrival. Execute refund via payment handler.

### Process Modifications

Check modification policy, apply any fees, and update the booking. If liability increases, the mandate may need re-signing.
::

---

## Learn More

::card-group
  ::card{title="Specification" to="/reference/folio/spec"}
  Full technical specification with schema definitions.
  ::
  ::card{title="Examples" to="/reference/folio/examples"}
  Working JSON examples for payment schedules and policies.
  ::
::
