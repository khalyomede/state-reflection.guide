---
extends: _layouts.main
section: body
title: Storage — State Reflection
description: A focus on storage strategies applied to State Reflection.
---

# Storage

State Reflection is flexible enough to adapt to many storage types.

Here are some advice to help you choose the best suitable storage for each components.

## Reflection

When an Action starts, you must store a representation of this Command (e.g. a Reflection).

Avoid temporary data storage types like In-Memory data storage systems (unless they offer a persisting option, like Redis AOF mode).

Prefer immutable data types, like WORM databases.

**Mostly advised**: SQL-based data types, or any other data storage type that supports commit/rollback mecanism

## Reflection timing

As reflections need to be attached to a timing, Timeseries databases are very well suited since they automatically attach the timing for you.

TimescaleDB may be considered as it is a PostgreSQL extension, which means your server can handle both Reflection storage (TimescaleDB) and State (classic PostgreSQL).

If you do not use a Timeseries database to store your Reflection, make sure to **store a timing** with the most precision possible. Prefer nanoseconds when possible.

## State

As they will be queried a lot more, favor data storage with high read throughput.

State are the data you return to your surfaces, so choose the data storage that is best suited for your uses cases:

- In-Memory for high frequency access to data that do not change often (maximizing cache hit)
- NoSQL for high frequency writes if you have a lot of Queries
- SQL if you need integrity constraints (very adviced as it provides the best guarantee for your State integrity)
- Graph/Geo database for more specific pathfinding scenarios
- Vector databases for deep relationship between data and recommendation/search systems
- ...

**Mostly advised**: SQL-based data types, or any other data storage type that supports commit/rollback mecanism.

[Previous: Explanation](../explanation)

[Next: Replay](../replay)

[Home](../)
