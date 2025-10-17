---
extends: _layouts.main
section: body
title: History — State Reflection
description: The origin story of why State Reflection was created.
---

# History

After trying Event Sourcing, and with a web background where MVC was ruling the interactions between users and the system, some issues arise quickly.

- The necessity to setup asynchronous mecanism (like Web Sockets or other Real Time solutions) to ensure users are notified when the Projections are finished
- The issues when a Projections is failing, given the eventual consistency nature that forces Events to be treated asynchronously to get the best out of Event Sourcing
- The difficulty to setup and maintain a Snapshot storage to optimize Projections computations
- The questions around how to deal with Events that resulted in failed Projections (given a user tries to pay his cart, what about the first failed event VS the second successful event? What happens in case the first event is replayed?)

## Flipping the steps

State Reflection in its purest form is simply "Event Sourcing backward".

Instead of treating Events (Command) as the first class citizen, we prefer to consider States (Query) as the primary ruler to determine if the Event (Reflection) should be stored.

This results in several advantages:

- You only store what is actually "successful"
- As a consequence, replaying Reflections (Event) will **always** guarantee the same State (Projection) if you do not change the State (which is not guaranteed with Event Sourcing as a consequence of the eventual consistency nature of this pattern)
- You no longer need to manage a Snapshot storage, since by definition State Reflection is snapshotted at every Reflection, speeding up the State computation and simplifying the storage architecture

And consequences:

- Since an Action must ensure the indempotency of a State and its Reflection, you may not be able to support asynchronous computation of State (Projection), which Event Sourcing allows by nature
- Actions may induces slowdowns since it is heavier to store both the State and the Reflection (for State Reflection) VS just the Event (for Event Sourcing), which may lead to difficulty to absorb high traffic scenarios when actions are heavy State computations
- Since only "successful" Actions are saved, you have to manually add audit Reflections yourself or anticipate future need, when Event Sourcing by definition is audit-ready by default since it stores the event right away before eventually computing Projections
- Rollback/Commit mecanism must be implemented to fully support State Reflection, which Event Sourcing does not need at all again thanks to its eventual consistency nature

## Trade offs

As you can see, State Reflection is not a "silver bullet" approach. It is just another approach to CQRS, that seems to be more natural for web developpers that are used to MVC or "classic" synchronous scenarios.

[⟵ Auditing](../auditing)

[⌂ Home](../)
