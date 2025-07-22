# Explicit Pushing of Information (`PUSH` / `PUSHING`)

:warning: **NOTE: This extension is discontinued, and will be removed in future versions.**

The default protocol specification allows pushing information at any time. Since the push is not mandatory the server has to be sure about the current status of the site. That means that on the approaching of the user or while payment the data for prices, products, and transactions are needed and will be requested. Some site systems may be able to provide the information on a push basis and can take over the responsibility.

In order for a site to take over the responsibility the client needs to send the capability `PUSH`. The server will directly after a new session is initiated, call the `PUSH` command to request which information can be pushed. The client then needs to respond with `PUSHING` and the pushed information. This way the client can decide and communicate that only a subset of information is pushed.

Pushing transactions is special, since `TRANSACTIONS` is still invoked in case of the Post Pay process, to not push all transactions every time a fueling takes place. Announcing `TRANSACTION` works only for pre auth transactions. Pushed transactions in post pay will be accepted but don't omit the call to the site.

**Important:** Every command that will we pushed need to be completely implemented as a capability. E.g. if transactions are pushed, the client still need to announce the `TRANSACTIONS` capability, if pumps are pushed the `PUMPS` capability is still required, same for `PRODUCTS`, `PRICES` and all other pushable commands.

## `PUSH`

Type: **Request/Response**

Sever requests details about which data will be pushed from the client.

If the client does not respond with `OK`, the session will be closed.

Arguments: **None**

Errors:

- **ANY** in case the PUSH can't be responded with `OK` the session will be terminated

## `PUSHING`

Type: **Notification**

Sends the capability that will be send during push. This transfers the responsibility to the client for the given method.

Arguments:

- **Capability** (arg0, string): Name of the capability that will be pushed by the client

## Example

```
C: * CAPABILITY CLEAR HEARTBEAT LOCKPUMP PRICES PUMPS PUMPSTATUS QUIT PRODUCTS TRANSACTIONS UNLOCKPUMP PUSH
S: * CAPABILITY BEAT CHARSET PLAINAUTH PRICE PUMP TRANSACTION QUIT LOCKEDPUMP RECEIPTINFO PUSHING
C: C0 CHARSET ISO-8859-1
S: C0 OK
C: C1 PLAINAUTH 9eb56d5e-6563-430a-9d39-5ddf567e73d5 1d3b755d3bce8f09b4f8ff08dabf1796
S: C1 OK
S: S0 PUSH
C: * PUSHING PRICE
C: * PUSHING PRODUCT
C: * PUSHING TRANSACTION
C: S0 OK
C: * PRICE 0100 LTR EUR 1.339 Super Plus
C: * PRICE 0200 LTR EUR 1.229 Super 95
C: * PRICE 0300 LTR EUR 1.499 Super 95 e5
C: * PRODUCT 0100 ron98 19.0
C: * PRODUCT 0200 ron95 19.0
C: * PRODUCT 0300 ron95e5 19.0
S: S2 PUMPSTATUS 3
C: * PUMP 3 locked
C: S2 OK
S: S3 PUMPSTATUS 3 30
C: * PUMP 3 free
C: S3 OK
C: * PUMP 3 in-use
C: * PUMP 3 ready-to-pay
S: S4 TRANSACTIONS
C: * TRANSACTION 3 c71b9838ad3dfc15 open 0100 EUR 86.83 72.978 19.0 13.65 LTR 54.40
C: S4 OK
S: S5 CLEAR 3 c71b9838ad3dfc15 e2f74ef5-f427-4ae6-bdd3-70a96709992f pace
C: * RECEIPTINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f Trace-No 008357
C: * RECEIPTINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f Terminal-ID 651199555
C: * RECEIPTINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f Receipt-No 7521
C: * RECEIPTINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f Local-Date 18.04.2020
C: * RECEIPTINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f Local-Time 09:37:30
C: * RECEIPTINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f VU-No 527
C: S5 OK
C: * QUIT bye bye
```
