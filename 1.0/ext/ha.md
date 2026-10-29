# Session High Availability Information (`SESSIONMODE`)

A client can connect the same site to the server through multiple connections/sessions. Based on the session priority (see [Connection Multiplexing](connection-multiplexing.md)) or, if none is given, the connection time, the server chooses the session with the highest priority (i.e. the lowest priority value, or the oldest connection) to communicate with the client. To tell the client exactly which communication path is used, a new message type `SESSIONMODE` is introduced.

## `SESSIONMODE`

Type: **Notification**

Direction: **Server → Client**

Informs the client about the state of the current connection/session. If the current connection/session is actively used, its state is "active"; if it is used as a fallback communication path, its state is "passive". If the client supports this message, the server sends it as a notification in response to a successful `PLAINAUTH` request, before returning `OK`. The state of the connection/session remains unchanged until the server sends a new `SESSIONMODE` message with a different state. If the currently "active" communication path is no longer available, the connection/session with the next highest priority is used and therefore receives a `SESSIONMODE` notification with the state "active".

Arguments:

- **State** (arg0, string): state of the current connection/session. Always one of: **active**, **passive**

## Example

```
C: * CAPABILITY CLEAR HEARTBEAT LOCKPUMP PRICES PUMPS PUMPSTATUS QUIT SESSIONMODE TRANSACTIONS UNLOCKPUMP
S: * CAPABILITY BEAT CHARSET SESSIONS NEWSESSION PLAINAUTH PRICE PUMP LOCKEDPUMP TRANSACTION QUIT
C: C1 NEWSESSION s1
S: C1 OK
C: C2 NEWSESSION s2
S: C2 OK
C: C3 NEWSESSION s3
S: C3 OK
C: s1.C1 PLAINAUTH 9eb56d5e-6563-430a-9d39-5ddf567e73d5 1d3b755d3bce8f09b4f8ff08dabf1796
S: s1.* SESSIONMODE active
S: s1.C1 OK
C: s2.C1 PLAINAUTH 6a3cc50e-dcbb-4d83-a67f-44b3728a00a6 9edbff7f89fcd1b54535f3636a066883
S: s2.* SESSIONMODE active
S: s2.C1 OK
C: s3.C1 PLAINAUTH 9eb56d5e-6563-430a-9d39-5ddf567e73d5 1d3b755d3bce8f09b4f8ff08dabf1796
S: s3.* SESSIONMODE passive
S: s3.C1 OK
S: s1.S1 PUMPS
C: s1.* PUMP 1 locked
C: s1.* PUMP 2 locked
C: s1.S1 OK
C: s1.* QUIT bye bye
S: s3.* SESSIONMODE active
S: s3.S2 PRICES
C: s3.* PRICE 0100 LTR EUR 1.339 Super Plus
C: s3.* PRICE 0200 LTR EUR 1.229 Super 95
C: s3.* PRICE 0300 LTR EUR 1.499 Super 95 e5
C: s3.* PRICE 0400 LTR EUR 1.209 Diesel
C: s3.S2 OK
```

## EBNF

```text
sessionmode_method = "SESSIONMODE"
	space string .
```
