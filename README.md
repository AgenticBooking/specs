# Agentic Booking

Domain semantics for AI agent discovery and booking in hospitality. Defines venues, booking terms, trust, and lifecycle — what Ai agents need to understand and book places to eat, drink, stay, and play.

Its much easier to view these at https://agenticbooking.org

---

## The New Opportunity

In 2026, finding places to eat, drink, stay or play starts with a question, not a search.

*"Find me a pub with rooms near Blakeney, the kind of place locals actually go, dog-friendly, nothing too formal."*

For an AI agent to answer that well, it needs structured ways to discover venues, understand what they're actually like, verify claims, and complete bookings. That's what these specs provide.

---

## Scope

These specs define hospitality semantics — what venues are, what bookings mean, how trust works.

They do not define:
- Payment processing
- Agent behaviour
- Discovery infrastructure
- Rendering

---

## The specs

### Curator

How agents find trusted sources.

Curator defines how authorities (DMOs, portfolios, editorial guides) express what they know about collections of venues.

Curator exists specifically because commerce protocols intentionally avoid defining trust, authority, and taste.

```
Agent: "Dog-friendly hotel in Norfolk?"
    │
    ▼
Agent queries Curators (Visit Norfolk, Great British Hotels)
    │
    ▼
Curators return venue references
    │
    ▼
Agent fetches Venue definitions
    │
    ▼
Agent evaluates fit, recommends, books
```

**[→ Curator specification](/building-blocks/curator)**

---

### Venue

What a hospitality venue publishes to be understood by AI agents.

```
Venue
├── Identity        Who this is, proof of control
├── Vibe            What it feels like
├── Attributes      Rooms, facilities, policies
├── Evidence        Claims with provenance
├── Fit             What it's good for (and not)
├── Units           Rooms, tables, experiences
├── Neighbourhood   What's nearby
├── Presentation    UI components
└── Answers         Quotable explanations
```

**[→ Venue specification](/building-blocks/venue)**

---

### Folio

The guest account. What's owed, what's paid, what happens if plans change.

Hospitality isn't instant commerce. A booking is an agreement about the future:

- Deposit now, balance at check-in
- Refund if cancelled, but less as date approaches
- Charge if no-show
- Fee if modified

```
Folio
├── Payment schedule    Deposit, balance, timing
├── Cancellation        Refund tiers by days before
├── No-show policy      When and how much
├── Modification terms  What changes cost
└── Mandate             Proof of agreement
```

**[→ Folio specification](/building-blocks/folio)**

---

### Stay

The booking lifecycle.

```
Request → Available → Held → Booked → Confirmed → Arrived → Stayed → Completed
```

Plus: `Modified`, `Cancelled`, `No-Show` as branch states.

**[→ Stay specification](/building-blocks/stay)**

---

### Bookable

The base pattern underlying Venue.

Venue is Bookable for hospitality. Bookable is the domain-agnostic pattern:

```
Bookable
├── Identity       Who this is
├── Evidence       Claims with provenance
├── Fit            What it's good for
├── Actions        What an agent can do
└── Answers        Quotable explanations
```

Could apply beyond hospitality.

**[→ Bookable specification](/building-blocks/bookable)**

---

## How they connect

```
┌───────────────────────────────────────────────────────────────┐
│                      CURATOR INDEX                             │
│           (Which Curators exist? Geographic routing)           │
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
              │   + Folio     │
              │   + Stay lifecycle    │
              └───────────────────────┘
```

**Discovery flows down.** Index → Curators → Venue references → Venues.

**Trust flows up.** Venue publishes → Curators verify → Convergence = confidence.

---

## Epistemic separation

Each layer speaks only to what it can credibly claim:

| Layer | Who speaks | What they claim |
|-------|------------|-----------------|
| **Venue** | The venue | "This is who I am" |
| **Curator** | The authority | "This is what I know about them" |
| **Folio** | The transaction | "This is what was agreed" |

Venues don't claim authority. Authorities don't claim to be venues. This separation is load-bearing.

---

## Protocol bindings

These specs define hospitality semantics. They are designed to be expressed over existing agent and commerce protocols, including:

| Protocol | Role |
|----------|------|
| [A2A](https://a2a-protocol.org/) / [MCP](https://modelcontextprotocol.io/) | Transport |
| [AP2](https://ap2-protocol.org/) | Payment authorization |
| [UCP](https://ucp.dev) | Commerce lifecycle orchestration |
| [ACP](https://stripe.com/docs/acp) | Agent coordination |
| [IEEE P3709](https://standards.ieee.org/) | Agent governance |
| REST | Direct HTTP |

The specs define WHAT hospitality commerce means. Protocols define HOW it moves.

### UCP integration

Google's [Universal Commerce Protocol](https://ucp.dev) launched January 2026 as an orchestration layer for agentic commerce. We're proposing these specs as a hospitality vertical:

| Our spec | UCP capability |
|----------|----------------|
| Venue | `dev.ucp.hospitality.venue` |
| Folio | `dev.ucp.hospitality.folio` |
| Stay | `dev.ucp.hospitality.stay` |

See [UCP Protocol](/reference/protocols/ucp) for details.

This is one binding, not the only one.

---

## Getting started

**If you're a venue** wanting to be AI-bookable:
→ Start with [Venue](/building-blocks/venue)

**If you're a DMO or authority** wanting to curate venues:
→ Start with [Curator](/building-blocks/curator)

**If you're integrating payments**:
→ Start with [Folio](/building-blocks/folio)

**If you're building agent tooling**:
→ Start with [Bookable](/building-blocks/bookable)

---

## Repository structure

```
/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
│
├── /introduction               # Overview, how it works, roadmap
│
├── /building-blocks            # Core concepts
│   ├── bookable.md             # Base pattern
│   ├── venue.md                # Hospitality identity
│   ├── curator.md              # Discovery layer
│   ├── stay.md                 # Booking lifecycle
│   └── folio.md                # Guest account
│
├── /trust                      # Trust & verification
│   ├── identity.md             # DIDs, ownership
│   ├── evidence.md             # Claims with provenance
│   ├── credentials.md          # Verifiable credentials
│   └── threat-model.md         # Security considerations
│
├── /audiences                  # Role-specific guides
│   ├── venues.md
│   ├── curators.md
│   ├── dmos.md
│   ├── agent-builders.md
│   └── enterprise.md
│
└── /reference                  # Detailed specs
    ├── /bookable
    ├── /venue
    ├── /curator
    ├── /stay
    ├── /folio
    ├── /identity
    └── /protocols              # A2A, UCP, AP2, ACP, P3709
```

---

## Status

| Spec | Version | Status |
|------|---------|--------|
| Curator | 0.2.0 | Draft |
| Venue | 0.1.0 | Draft |
| Folio | 0.1.0 | Draft |
| Stay | 0.1.0 | Draft |
| Bookable | 0.1.0 | Draft |

**License:** MIT

These are working specifications. They will evolve based on feedback and implementation experience.

---

## Contributing

Open an issue. Submit a PR. Tell us what's wrong or missing.

We're not precious about this. If something doesn't work, say so.

---

## About

These specifications are led by [Selfe](https://selfe.ai), building AI-native hospitality infrastructure.

[selfe.ai](https://selfe.ai) · [hello@agenticbooking.org](mailto:hello@agenticbooking.org)

Questions? [Open an issue](https://github.com/AgenticBooking/specs/issues) or email us.

---

<p align="center">
  <br />
  <em>Domain semantics for agentic booking</em>
  <br /><br />
  <a href="/introduction/overview">Get Started</a> · <a href="/introduction/roadmap">Roadmap</a> · <a href="./CONTRIBUTING.md">Contribute</a>
</p>
