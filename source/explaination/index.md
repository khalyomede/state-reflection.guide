---
extends: _layouts.main
section: body
title: Explaination — State Reflection
description: State Reflection principles explained.
---

# Explaination

A command must be followed by at least one (or more) query.

If an error occurs during the command or the query, the whole Action is a failure and must be canceled (e.g. rollback of stored queries and the command representation).

## CQRS

State Reflection is implementing CQRS principles.

## Life cycle

1. A command is received
2. The current State is snapshotted
2. Queries produce results (e.g. computing a new State from the current State) which is stored
3. A representation of the command (reflection) is stored
4. Queries results (new State) are returned in response

The term "Action" will be used to represent all these steps.

## State

State represent the queries that computed a new state from a previous state.

## Reflection

A reflection is an event that contains all the data that represent a received Command.

It must be persisted in a (ideally immutable append-only) storage system.

Queries react to the Command to compute the State. Reflections simply mirror the Commands for later replay.

## Action Atomicity

When an Action begins, it must snapshot the current State, and only use this State to compute query results.

At any step of the life cycle of the Action, if any error occurs, the whole Action is cancelled.

Anything that prevents the response to be sent successfully, or any event that endanger the storage of the command representation (Reflection) or the query results (new State) is considered an error.

When an Action is cancelled, the State **must be** the exact same as before the Action occured. As a result, the snapshotted State must be found after the Action is cancelled.

The Action is indempotent by consequence.

## Command storage timing

Commands may happen "simultaneously" or "in parallel", they all get assigned the most precise timing possible.

Reflection must be accompanied by this timing.

## State immutability

Once a command is stored, it **must not** be modified.

## Command replay

As reflections are immutable, they can be replayed.

As reflections are stored with their timing, they **must** be replay in the timing order.

As command are replayed, their State is de facto the exact same as when the Action was first performed (thanks to the State snapshotting occuring during before Action started).

## State immutability

State Reflection ensure that when queries are not modified, replaying reflections at any given time will produce the exact same State, no matter how many time reflections are replayed.

[⟵ Introduction](../introduction)

[Storage ⟶](../storage)

[⌂ Home](../)
