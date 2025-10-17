---
extends: _layouts.main
section: body
title: Auditing — State Reflection
description: Explaination on how Stateless Reflection can unlock auditing capabilities.
---

# Auditing

As mentioned, an Action ensures only valid Reflections (commands) are saved, by ensuring the whole stack of State (Queries) is successful.

However, your system may (for business reasons) not have to listen to every single entrypoint.

## Stateless Reflections

For example, if we take back the example of the shoe store, your login controller may just update the Cookies (to save to session) and store no information.

In this case, you may create "Stateless" reflections, which are just Commands without any Queries.

Also called "Empty Actions", Stateless Reflections are useful to just store Commands, for later analyzis.

You may also use Stateless Reflections to anticipate future need, and avoid loosing data.

Still on our Shoe store login example, you may decide to start right away with storing a Reflection "UserLoggedIn", which would perform no queries right now.

One day you may decide to display the list of "currently logged devices". Only at this moment you would attach a State (SaveLoggedUserState) listener to the "UserLoggedIn" event, and simply replay the history of State Reflections to update all your users's surfaces to show currently logged devices.

## Security Audit

In the context of a web app, you may want to store any requests (POST, DELETE, GET, ...) as a Reflection. This would allow you to:

- Either perform security audit on a regular basis, by scanning the users' access behaviors
- Apply some rate limiting
- Setup some alerting threshold (for example, on the number of times a resource is access)
- ...

As a general advice, these "Audit Reflections" would need to save the full context of the HTTP request (headers, queries, POST body, ...). Consider using a compressed format to save the Reflection payload (like MsgPack if your storage layer supports binary columns).

[Previous: Example](../example)

[Next: History](../history)

[Home](../)
