# TRANSACTIONINFO (`TRANSACTIONINFO`)

A client may need the TRANSACTIONINFO in case of payment by card. To signal the server that the TRANSACTIONINFO is required, the new message type `TRANSACTIONINFO` is introduced. If the `TRANSACTIONINFO` capability is announced by the client, the server will send the Info for the transaction before clearing (`CLEAR`) and before (`UNLOCKPUMP`) in case of pre-auth. Since those information are very sensitive, the OpenFSC server can neglect to send the TRANSACTIONINFO even if requested by the client. Eligibility for the TRANSACTIONINFO is stored in the OpenFSC backend and has to be agreed on business-wise.

## `TRANSACTIONINFO`

Type: **Notification**

Prepares the PAN, ApprovalCode and Mileage information to execute a later CLEAR or a UNLOCKPUMP.

Arguments:

- **FSCTransactionID** (arg0, uuid): ID given by the server for informational reasons (defined by the server).

- **<PAN/ApprovalCode/Mileage>** (arg1, string): Key following by the corresponding value

- **PAN** (arg2, string): PAN is the 19 chars primary account number
- **ApprovalCode** (arg2, string): ApprovalCode is the 6 chars approval code number
- **Mileage** (arg2, string): Mileage is the 6 chars number

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
S: * TRANSACTIONINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f PAN 92873498723498792384
S: * TRANSACTIONINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f ApprovalCode 643972
S: * TRANSACTIONINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f Mileage 176439
S: S6 CLEAR 3 c71b9838ad3dfc15 e2f74ef5-f427-4ae6-bdd3-70a96709992f pace
C: S6 OK
C: * QUIT bye bye
```

## Example Pre Auth

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
C: * PUMP 1 locked
C: * PUMP 2 free
C: * PUMP 3 free
C: * PUMP 4 free
C: S1 OK
S: * TRANSACTIONINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f PAN 92873498723498792384
S: * TRANSACTIONINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f ApprovalCode 643972
S: * TRANSACTIONINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f Mileage 176439
S: S2 UNLOCKPUMP 1 EUR 10.0 70644955-ef32-4d33-a88b-67b500a7c00d pace
C: S2 OK
S: S3 PUMPS
C: * PUMP 1 in-use
C: * PUMP 2 free
C: * PUMP 3 free
C: * PUMP 4 free
C: S3 OK
S: S4 PUMPSTATUS 1 30
C: * PUMP 1 free
C: S4 OK
S: S5 PUMPSTATUS 1 30
C: * PUMP 1 in-use
C: S5 OK
S: S6 TRANSACTIONS 1
C: * TRANSACTION 1 70644955-ef32-4d33-a88b-67b500a7c00d open 0100 EUR 5.50 4.62 19.00 0.88 LTR 4.37 1.258
C: S6 OK
S: S7 CLEAR 1 70644955-ef32-4d33-a88b-67b500a7c00d 70644955-ef32-4d33-a88b-67b500a7c00d pace
C: S7 OK
C: * QUIT bye bye
```
