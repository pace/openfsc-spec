# PAN (`PAN`)

**NOTE: This extension is discontinued, [Transaction Information](transaction-info.md) should be used instead.**

A client may need the PAN for clearing. To signal the server that a PAN is required, the new message type `PAN` is introduced. If the `PAN` capability is announced by the client, the server will send the PAN for the transaction before clearing (`CLEAR`). Since a PAN is a very sensitive piece of information, the OpenFSC server can neglect to send the PAN even if requested by the client. Eligibility for the PAN is stored in the OpenFSC backend and has to be agreed on business-wise.

## `PAN`

Type: **Request/Response**

Prepares the PAN information to execute a later CLEAR.

Arguments:

- **FSCTransactionID** (arg0, uuid): ID given by the server for informational reasons (defined by the server). Will be returned with the corresponding `CLEAR` command, to correlate the transactions.

- **PAN** (arg1, string): PAN is the 19 chars primary account number

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
C: * TRANSACTION 3 c71b9838ad3dfc15 open 0100 EUR 86.83 72.978 19.0 13.65 LTR 54.40
C: S4 OK
S: S5 PAN e2f74ef5-f427-4ae6-bdd3-70a96709992f 654654654654654488
C: S5 OK
S: S6 CLEAR 3 c71b9838ad3dfc15 e2f74ef5-f427-4ae6-bdd3-70a96709992f pace
C: S6 OK
C: * QUIT bye bye
```

PAN should be deleted from session store on client side after successful or transaction failed. In case the PAN is requierd the client is allowed to return the error `404` on clearing:

```
S: S6 CLEAR 3 c71b9838ad3dfc15 e2f74ef5-f427-4ae6-bdd3-70a96709992f pace
C: S6 ERR 404 PAN not found for transaction id
```
