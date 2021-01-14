---
title: utf8-only capability
layout: spec
meta-description: A capability that indicates only UTF-8 traffic is allowed.
copyrights:
  -
    name: "Daniel Oaks"
    period: "2021"
    email: "daniel@danieloaks.net"
  -
    name: "Shivaram Lingamneni"
    period: "2021"
    email: "slingamn@cs.stanford.edu"
---

## Notes for implementing work-in-progress version
This is a work-in-progress specification. Software implementing this work-in-progress specification MUST NOT use the unprefixed `utf8-only` capability name. Instead, implementations MUST use the `draft/utf8-only` capability name to be interoperable with other software implementing a compatible work-in-progress version.


## Introduction
IRC predates the Unicode standard. Consequently, although UTF-8 has been widely adopted on IRC, clients cannot assume that all IRC data is UTF-8. This specification defines an informational capability for servers that only allow UTF-8 on their network, letting clients change their encoding to suit.

## The `draft/utf8-only` capability
`draft/utf8-only` is an informational capability indicating that the server only supports UTF-8 messages. Servers advertising this capability MUST NOT relay content (such as `PRIVMSG` or `NOTICE` message data, channel topics, or realnames) containing non-UTF-8 data to clients. Clients implementing this specification MUST NOT send non-UTF-8 data to the server once they have seen this capability. Server handling of such messages is implementation-defined, and MAY involve sending the `FAIL` code described below.

If a client implementing this specification sees this capability, they MUST set their outgoing encoding to UTF-8 without requiring any user intervention. This allows clients to work transparently on networks that only allow UTF-8 traffic.

## The `INVALID_UTF8` `FAIL` code
This is a code that can be used with the `FAIL` command, as defined by the [standard replies](https://ircv3.net/specs/extensions/standard-replies) specification. This code indicates to the client that their message was dropped or modified because it contained non-UTF-8 bytes.

## Example

```
Client: PRIVMSG #ircv3 :<non-utf-8 message>
Server: FAIL PRIVMSG INVALID_UTF8 :Message rejected, your IRC software MUST use UTF-8 encoding on this network
```
