# PAN (`PAN`)

:warning: **NOTE: This extension is discontinued, [Transaction Information](transaction-info.md) should be used instead.**

A client may need the PAN for clearing. To signal to the server that a PAN is required, the new message type `PAN` is introduced. If the client announces the `PAN` capability, the server sends the PAN of the transaction before clearing it (`CLEAR`). Since a PAN is very sensitive information, the OpenFSC server may decline to send the PAN even if the client requests it. Eligibility for receiving the PAN is stored in the OpenFSC backend and has to be agreed upon on a business level.

## `PAN`

Type: **Request/Response**

Direction: **Server → Client**

Provides the PAN needed for a subsequent `CLEAR`.

Arguments:

- **FSCTransactionID** (arg0, uuid): ID defined by the server. The same ID is sent with the corresponding `CLEAR` request to correlate the transactions.

- **PAN** (arg1, string): primary account number (19 to 21 digits)

## Example

```
C: * CAPABILITY CLEAR HEARTBEAT LOCKPUMP PRICES PUMPS PUMPSTATUS QUIT TRANSACTIONS UNLOCKPUMP PAN
S: * CAPABILITY BEAT CHARSET PLAINAUTH PRICE PUMP LOCKEDPUMP TRANSACTION QUIT
C: C0 CHARSET ISO-8859-1
S: C0 OK
C: C1 PLAINAUTH 9eb56d5e-6563-430a-9d39-5ddf567e73d5 1d3b755d3bce8f09b4f8ff08dabf1796
S: C1 OK
S: S0 PRICES
C: * PRICE 0100 LTR EUR 1.339 Super Plus
C: * PRICE 0200 LTR EUR 1.229 Super 95
C: * PRICE 0300 LTR EUR 1.499 Super 95 e5
C: S0 OK
S: S1 PUMPS
C: * PUMP 1 in-use
C: * PUMP 2 out-of-order
C: * PUMP 3 free
C: * PUMP 4 ready-to-pay
C: S1 OK
S: S2 PUMPSTATUS 3
C: * PUMP 3 free
C: S2 OK
S: S3 PUMPSTATUS 3 30
C: * PUMP 3 free
C: S3 OK
C: * PUMP 3 in-use
C: * PUMP 3 ready-to-pay
S: S4 TRANSACTIONS
C: * TRANSACTION 3 c71b9838ad3dfc15 open 0100 EUR 86.83 72.97 19.0 13.86 LTR 64.85 1.339
C: S4 OK
S: S5 PAN e2f74ef5-f427-4ae6-bdd3-70a96709992f 6546546546546544881
C: S5 OK
S: S6 CLEAR 3 c71b9838ad3dfc15 e2f74ef5-f427-4ae6-bdd3-70a96709992f pace
C: S6 OK
C: * QUIT bye bye
```

The client should delete the PAN from its session store once the transaction has either succeeded or failed. If the PAN is required but was not received, the client may return the error `404` on clearing:

```
S: S6 CLEAR 3 c71b9838ad3dfc15 e2f74ef5-f427-4ae6-bdd3-70a96709992f pace
C: S6 ERR 404 PAN not found for transaction id
```
