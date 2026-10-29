# Explicit Pushing of Information (`PUSH` / `PUSHING`)

:warning: **NOTE: This extension is discontinued, and will be removed in future versions.**

The base protocol allows clients to push information at any time. Since pushing is not mandatory, the server cannot rely on pushed data to know the current state of the site. Therefore, prices, products and transactions are requested by the server when a user approaches the site or during payment. Some site systems are able to push this information reliably and can take over this responsibility.

For a site to take over this responsibility, the client needs to announce the `PUSH` capability. Directly after a new session has been established, the server calls `PUSH` to request which information will be pushed. The client then responds with a `PUSHING` notification for each type of information it pushes. This way, the client can decide to push only a subset of the information.

Pushing transactions is a special case: in the Post-Pay process, the server still requests transactions with `TRANSACTIONS`, so that the client does not have to push all transactions every time a fueling takes place. Announcing `TRANSACTION` only takes effect for Pre-Auth transactions. Pushed Post-Pay transactions are accepted, but the server still requests the transactions from the site.

**Important:** Every method whose data is pushed must still be fully implemented as a capability. E.g. if transactions are pushed, the client still needs to announce the `TRANSACTIONS` capability; if pumps are pushed, the `PUMPS` capability is still required. The same applies to `PRODUCTS`, `PRICES` and all other pushable methods.

## `PUSH`

Type: **Request/Response**

Direction: **Server → Client**

The server requests details about which data will be pushed by the client.

If the client does not respond with `OK`, the session will be closed.

Arguments: **None**

Errors:

- **Any** error: if the `PUSH` request is not answered with `OK`, the session will be terminated

## `PUSHING`

Type: **Notification**

Direction: **Client → Server**

Announces a method whose data will be pushed by the client. This transfers the responsibility for that data to the client.

Arguments:

- **Capability** (arg0, string): name of the method whose data will be pushed by the client

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
C: * PRICE 0200 LTR EUR 1.229 Super 95 E10
C: * PRICE 0300 LTR EUR 1.499 Super 95 e5
C: * PRODUCT 0100 ron98 19.0
C: * PRODUCT 0200 ron95e10 19.0
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
C: * TRANSACTION 3 c71b9838ad3dfc15 open 0100 EUR 86.83 72.97 19.0 13.86 LTR 64.85 1.339
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
