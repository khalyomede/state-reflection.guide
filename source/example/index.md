---
extends: _layouts.main
section: body
title: Example — State Reflection
description: An everyday scenario to understand State Reflection in practice.
---

# Example

Say you are building a shoe store.

Your web app allows to add items to a cart, and to proceed with the checkout.

Let us focus on the checkout process.

## Checkout controller

Here is the pseudo-algorithm that would be done for a classic MVC app.

```
cart checkout request received
├── Finding cart by request parameter id
├── Calling Payment Gateway to capture the cart total amount
├── Cart status moved to "paid" in database
├── Successful payment data attached to cart
└── Redirect response return
```

## Using State Reflection

Here is the rewritten controller using State Reflection

```
cart checkout request received
  finding cart in database by request parameter id
  Calling Payment Gateway to capture the cart total amount
  A "cart paid" action is begining
    State "Cart status moved to paid" stored
    State "Successful payment data" stored
    Reflection stored
  Redirect response return
```

We defer the Action to the very end of the controller to avoid most of the side effects (like calling an HTTP endpoint to perform the payment).

Also notice the order of events:

1. We save the States
2. We persist the Reflection

This is to ensure **only successful** State are reflected.

As a side note, if both your State and Reflection storage types supports rollbacks, you may move steps at any moment of your Action (in the order of your choice), and even before your side effects.

```
cart checkout request received
  finding cart by request parameter id
  -- Begin of transaction --
  A "cart paid" action is begining
    Reflection stored
    State "Cart status moved to paid" stored
    State "Successful payment data" stored
  Calling Payment Gateway to capture the cart total amount
  -- Transaction commit --
  Redirect response return
```

This may be required when you need to attach cart related meta data to your payment request, although in this case it would be preferable to use a UUID that you can generate or retreive, performing your side effects, and only then running your Action.

[⟵ Example](../explaination)

[History ⟶](../history)

[⌂ Home](../)
