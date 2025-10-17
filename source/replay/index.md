---
extends: _layouts.main
section: body
title: Replay — State Reflection
description: A way to rewind and re-run events.
---

# Replay

Reflections are mirroring the actual Command that originally occured.

Replaying reflections simply means:

- Erasing the State
- Ordering reflections by timing
- Running reflection (e.g. Command)
- State rewriting

## Motivations

Replaying have two specific goals:

- Allowing you to change your queries, to suit new or evolving surfaces needs
- Allowing you to change your State data storage scheme easily
- Allowing you to debug by going at a specific point in time

## Advice

As you must ensure the same State is built no matter if the Command was played from the original event or a Reflection replay, Event/Listener or Pub/Sub pattern works best.

Given this Event and its listeners

```
CartPaidEvent
  StoreCartPaidReflection
  StoreCartPaidState
  StoreCartPaymentState
```

You would want to dispatch the event from both your controller and your replay command.

[⟵ Storage](../storage)

[Example ⟶](../example)

[⌂ Home](../)
