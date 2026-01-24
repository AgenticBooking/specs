---
title: Settlement
description: Commercial roles in agentic bookings—who receives payment, who provides service, and how liability flows between parties.
order: 3
---

# Settlement

::badge{type="warning"}
v0.1.0 Draft
::

Commercial roles and payment flows in multi-party bookings.

**Namespace URI:** `https://agenticbooking.org/folio/settlement/v1`

---

## 1. Overview

When a guest books through an agent, multiple parties are involved:

```
Guest → Agent → Venue
         ↓
      Platform
```

Each party has a commercial role that determines who receives payment, who provides service, and who handles disputes.

::callout{type="info"}
This specification defines the vocabulary for declaring commercial roles. It does not mandate any particular payment provider or implementation.
::

---

## 2. Commercial Roles

### 2.1 Role Types

| Role | Description | Responsibilities |
|------|-------------|------------------|
| `merchant_of_record` | The legal seller | Receives payment, appears on statements, handles disputes |
| `service_provider` | Delivers the service | Provides the stay, meal, or experience |
| `facilitator` | Platform infrastructure | Connects parties, provides tooling, no customer liability |

### 2.2 Role Declaration

```json
{
  "settlement": {
    "merchant_of_record": {
      "party": "agent",
      "id": "did:web:travel.example.com"
    },
    "service_provider": {
      "party": "venue",
      "id": "did:web:theroste.co.uk"
    },
    "facilitator": {
      "party": "platform",
      "id": "did:web:platform.example.com"
    }
  }
}
```

### 2.3 Party Values

| Value | Meaning |
|-------|---------|
| `venue` | The hotel, restaurant, or experience provider |
| `agent` | The AI agent or travel service booking on behalf of the guest |
| `platform` | Infrastructure provider connecting agents and venues |
| `ota` | Online travel agency acting as intermediary |

---

## 3. Settlement Models

How funds flow depends on who is Merchant of Record:

### 3.1 Venue Collects

Traditional model—venue receives payment directly.

```
Guest → Venue (MoR)
          ↓ commission
        Agent
```

```json
{
  "settlement": {
    "model": "venue_collects",
    "merchant_of_record": { "party": "venue" }
  }
}
```

### 3.2 Agent Collects

Agent receives payment, remits to venue.

```
Guest → Agent (MoR)
          ↓ payout
        Venue
```

```json
{
  "settlement": {
    "model": "agent_collects",
    "merchant_of_record": { "party": "agent" }
  }
}
```

### 3.3 Model Comparison

| Aspect | Venue Collects | Agent Collects |
|--------|----------------|----------------|
| Statement shows | Venue name | Agent name |
| Refund authority | Venue | Agent |
| Chargeback liability | Venue | Agent |
| Guest relationship | Direct with venue | Through agent |

---

## 4. Refund Authority

Who can initiate and execute refunds depends on MoR designation.

### 4.1 Authority Chain

```json
{
  "refund_authority": {
    "initiators": ["guest", "agent", "venue"],
    "executor": "merchant_of_record",
    "requires_authorization": true
  }
}
```

| Field | Description |
|-------|-------------|
| `initiators` | Parties who can request a refund |
| `executor` | Party who executes the refund (always MoR) |
| `requires_authorization` | Whether initiator needs MoR approval |

### 4.2 Refund Scenarios

| Scenario | Initiator | Executor | Cost Allocation |
|----------|-----------|----------|-----------------|
| Guest cancels (within policy) | Guest | MoR | Per cancellation tier |
| Guest cancels (outside policy) | Guest | MoR | Guest forfeits per policy |
| Venue cannot fulfil | Venue | MoR | Venue reimburses |
| Agent error | Agent | MoR | Agent absorbs |
| Force majeure | Any | MoR | Per terms or negotiated |

### 4.3 Cost Allocation

```json
{
  "refund_cost_allocation": {
    "guest_cancellation": "per_policy",
    "venue_failure": "venue_reimburses",
    "agent_error": "agent_absorbs",
    "force_majeure": "negotiated"
  }
}
```

---

## 5. Liability Chain

Consumer protection responsibilities follow the MoR.

### 5.1 Consumer Responsibilities

| Responsibility | Falls to |
|----------------|----------|
| Cooling-off rights | MoR |
| Cancellation terms | MoR |
| Refund timelines | MoR |
| Service quality claims | MoR (may seek recourse from venue) |
| Chargeback defence | MoR |

### 5.2 Declaration

```json
{
  "liability": {
    "consumer_contract": "merchant_of_record",
    "service_delivery": "service_provider",
    "dispute_first_contact": "merchant_of_record"
  }
}
```

---

## 6. Multi-Party Bookings

Itineraries spanning multiple venues:

```json
{
  "settlement": {
    "model": "agent_collects",
    "merchant_of_record": { "party": "agent" },
    "service_providers": [
      { "party": "venue", "id": "did:web:hotel.example.com", "portion": 60 },
      { "party": "venue", "id": "did:web:restaurant.example.com", "portion": 25 },
      { "party": "venue", "id": "did:web:tour.example.com", "portion": 15 }
    ]
  }
}
```

The MoR receives full payment and distributes to service providers per agreed terms.

---

## 7. Payment Token Requirements

When using payment tokens (SPT, AP2 mandates), hospitality requires:

| Requirement | Description | Standard Support |
|-------------|-------------|------------------|
| `valid_from` | Don't charge before date | Needed for balance payments |
| `valid_until` | Don't charge after date | Supported |
| `max_amount` | Maximum chargeable | Supported |
| `condition` | Charge only if condition met | Needed for no-show |

::callout{type="warning"}
Current SPT implementations support `expires_at` but not `valid_from`. This gap is being addressed in hospitality protocol extensions.
::

See [ACP Protocol](/reference/protocols/acp#spt-hospitality-requirements) for details.

---

## 8. Implementation Notes

### What This Spec Defines

- Vocabulary for commercial roles
- Declaration format for settlement models
- Refund authority semantics
- Liability allocation patterns

### What This Spec Does Not Define

- Payment provider APIs
- Fee structures or percentages
- Account onboarding flows
- Transaction reconciliation

These are implementation concerns left to platforms and payment providers.

---

## Related Specifications

- [Folio Spec](/reference/folio/spec) — Payment schedules and policies
- [AP2 Protocol](/reference/protocols/ap2) — Mandate authorization
- [ACP Protocol](/reference/protocols/acp) — Checkout and SPT

---

::callout{type="info"}
Settlement is an open specification under MIT license.
::
