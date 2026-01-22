# Agentic Booking Specifications

GitHub-readable versions of the Agentic Booking specifications.

> For the full documentation site with navigation and interactive examples, visit **[agenticbooking.org](https://agenticbooking.org)**

---

## Specifications

| Spec | Description | Examples |
|------|-------------|----------|
| [Bookable](./bookable/spec.md) | Base pattern for AI-bookable entities | — |
| [Venue](./venue/spec.md) | Hospitality extension—hotels, restaurants, bars | [Examples](./venue/examples.md) |
| [Curator](./curator/spec.md) | Discovery and trust layer | [Examples](./curator/examples.md) |
| [Curator Principles](./curator/principles.md) | Public commitments for curators | — |
| [Stay](./stay/spec.md) | Booking lifecycle state machine | [Examples](./stay/examples.md) |
| [Folio](./folio/spec.md) | Payment semantics | [Examples](./folio/examples.md) |

---

## Quick Start

**If you're a venue** wanting to be AI-bookable:
→ Start with [Venue](./venue/spec.md)

**If you're a DMO or authority** wanting to curate venues:
→ Start with [Curator](./curator/spec.md)

**If you're integrating payments**:
→ Start with [Folio](./folio/spec.md)

**If you're building agent tooling**:
→ Start with [Bookable](./bookable/spec.md)

---

## How They Connect

```
┌───────────────────────────────────────────────────────────────┐
│                      CURATOR INDEX                            │
│           (Which Curators exist? Geographic routing)          │
└───────────────────────────┬───────────────────────────────────┘
                            │
         ┌──────────────────┼──────────────────┐
         ▼                  ▼                  ▼
  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
  │Visit Norfolk│    │  Cottages   │    │  Good Food  │
  │   (DMO)     │    │ (Portfolio) │    │ (Editorial) │
  └──────┬──────┘    └──────┬──────┘    └──────┬──────┘
         │                  │                  │
         └──────────────────┼──────────────────┘
                            │
                            ▼
              ┌───────────────────────┐
              │        VENUE          │
              │   + Folio (payment)   │
              │   + Stay (lifecycle)  │
              └───────────────────────┘
```

**Discovery flows down.** Index → Curators → Venue references → Venues.

**Trust flows up.** Venue publishes → Curators verify → Convergence = confidence.

---

## Status

| Spec | Version | Status |
|------|---------|--------|
| Bookable | 0.1.0 | Draft |
| Venue | 0.1.0 | Draft |
| Curator | 0.2.0 | Draft |
| Stay | 0.1.0 | Draft |
| Folio | 0.1.0 | Draft |

**License:** MIT

---

## Contributing

Open an issue. Submit a PR. Tell us what's wrong or missing.

[GitHub Issues](https://github.com/AgenticBooking/specs/issues) · [Email](mailto:hello@selfe.ai)
