---
title: oragono.io/utf8-only capability
layout: spec
meta-description: A capability that indicates only UTF-8 traffic is allowed.
copyrights:
  -
    name: "Daniel Oaks"
    period: "2021"
    email: "daniel@danieloaks.net"
---
## Notes for implementing work-in-progress version
This is a work-in-progress specification.

Software implementing this work-in-progress specification MUST NOT use the unprefixed `utf8-only` capability name. Instead, implementations MUST use the `oragono.io/utf8-only` capability name to be interoperable with other software implementing a compatible work-in-progress version.


## Introduction
IRC encodings have been an issue for a very long time. Some servers decide to disallow all encodings other than UTF-8 to prevent interoperability issues between clients. This specification defines an informational cap and a `FAIL` code to assist clients while connected to servers that do this.

## The `oragono.io/utf8-only` capability
This capability indicates that the server only supports UTF-8 messages. This means that any messages sent to the client from the server (and from any other clients) will be UTF-8. Servers that receive non-UTF-8 traffic from clients may handle it in an implementation-defined way, but MUST NOT send messages containing non-UTF-8 data to any other clients on the network.

If a client receives this informational cap, they MUST silently set their outgoing encoding to UTF-8 without any user intervention. This allows misconfigured clients to transparently work on networks that only allow UTF-8 traffic.

## The `INVALID_UTF8` `FAIL` code
This is a code that can be used with the `FAIL` command, as defined by the [standard replies](https://ircv3.net/specs/extensions/standard-replies) specification. This code indicates to the client that their message was dropped or modified because it contained non-UTF-8 bytes.

## Example

```
Client: PRIVMSG #ircv3 :<non-utf-8 message>
Server: FAIL PRIVMSG INVALID_UTF8 :Message rejected, your IRC software MUST use UTF-8 encoding on this network
```
