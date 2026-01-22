---
title: What is this?
order: 1
---

# What is Agentic Booking?

Domain semantics for AI agent discovery and booking in hospitality.

---

Agentic Booking extends [A2A (Agent-to-Agent)](https://google.github.io/A2A/) and [UCP (Universal Commerce Protocol)](https://www.ucprotocol.org/) for the hospitality vertical. It defines what AI agents need to understand and book places to eat, drink, stay, and play.

## The problem

AI agents are getting good at conversation. But when it comes to actually *doing* things—booking a hotel, reserving a table, finding the right venue—they hit a wall.

::note
Every venue has different systems. Different APIs. Different ways of describing what they offer. An agent that can book one hotel can't necessarily book another.
::

## The solution

A shared vocabulary. Open specs that define:

::card-group{cols=3}
  ::card{title="What venues publish" icon="ph:building"}
  So agents can understand them
  ::

  ::card{title="How bookings work" icon="ph:calendar-check"}
  So agents can make them
  ::

  ::card{title="Who to trust" icon="ph:shield-check"}
  So agents know which venues are legitimate
  ::
::

## The five protocols

::card-group{cols=2}
  ::card{title="Venue" icon="ph:map-pin" href="/protocols/venue"}
  What a hospitality venue publishes to be understood by AI agents.
  ::

  ::card{title="Bookable" icon="ph:calendar" href="/protocols/bookable"}
  The base pattern for anything that can be reserved.
  ::

  ::card{title="Folio" icon="ph:wallet" href="/protocols/folio"}
  The guest account. Payments, charges, refunds.
  ::

  ::card{title="Stay" icon="ph:bed" href="/protocols/stay"}
  The booking lifecycle from request to checkout.
  ::

  ::card{title="Curator" icon="ph:sparkle" href="/protocols/curator"}
  Discovery and trust. How agents find and verify venues.
  ::
::

## Built on standards

This isn't a new protocol from scratch. It's an extension layer:

::card-group{cols=2}
  ::card{title="A2A (Agent-to-Agent)" icon="ph:plugs-connected" href="https://google.github.io/A2A/"}
  Google's standard for agent communication. Defines how agents discover each other and exchange messages.
  ::

  ::card{title="UCP (Universal Commerce Protocol)" icon="ph:shopping-cart" href="https://www.ucprotocol.org/"}
  The commercial transaction layer. Handles payments, lifecycle, and business logic.
  ::
::

::tip
We add the booking-specific semantics on top. Venue types, booking patterns, trust hierarchies—the vocabulary agents need for hospitality.
::
