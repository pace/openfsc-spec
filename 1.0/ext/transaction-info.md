# Transaction Information (`TRANSACTIONINFO`)

A client may need additional transaction information, e.g. in case of payment by card. To signal to the server that it accepts this information, the client announces the `TRANSACTIONINFO` capability. If it does, the server sends the information for the transaction before clearing it (`CLEAR`) or, in the Pre-Auth process, before unlocking the pump (`UNLOCKPUMP`).

**Since this information is very sensitive, the OpenFSC server may decline to send `TRANSACTIONINFO` even if the client accepts it. Eligibility for receiving `TRANSACTIONINFO` is stored in the OpenFSC backend and has to be agreed upon on a business level.**

## `TRANSACTIONINFO`

Type: **Notification**

Direction: **Server → Client**

Provides transaction information such as the PAN, approval code and mileage, needed for a subsequent `CLEAR` or `UNLOCKPUMP`.

Arguments:

- **FSCTransactionID** (arg0, uuid): ID defined by the server.
- **Key** (arg1, string): key of the information (see table below)
- **Value** (arg2, string): value of the information; its format depends on the key

Defined key/value pairs:

|Key|Type|Description|
|-|-|-|
|`PAN`|string|Primary account number (19 to 21 digits) of the card used for this transaction|
|`ApprovalCode`|number|6-digit approval code (IFSF H2H)|
|`Mileage`|number|Odometer reading of the vehicle, up to 7 digits (unit: kilometers). Default value: "0"|

## Example (Post-Pay)

```
C: * CAPABILITY CLEAR HEARTBEAT LOCKPUMP PRICES PUMPS PUMPSTATUS QUIT TRANSACTIONS UNLOCKPUMP TRANSACTIONINFO
S: * CAPABILITY BEAT CHARSET PLAINAUTH PRICE PUMP LOCKEDPUMP TRANSACTION TRANSACTIONINFO QUIT
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
S: * TRANSACTIONINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f PAN 92873498723498792384
S: * TRANSACTIONINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f ApprovalCode 643972
S: * TRANSACTIONINFO e2f74ef5-f427-4ae6-bdd3-70a96709992f Mileage 176439
S: S6 CLEAR 3 c71b9838ad3dfc15 e2f74ef5-f427-4ae6-bdd3-70a96709992f pace
C: S6 OK
C: * QUIT bye bye
```

## Example (Pre-Auth)

```
C: * CAPABILITY CLEAR HEARTBEAT LOCKPUMP PRICES PUMPS PUMPSTATUS QUIT TRANSACTIONS UNLOCKPUMP TRANSACTIONINFO
S: * CAPABILITY BEAT CHARSET PLAINAUTH PRICE PUMP LOCKEDPUMP TRANSACTION TRANSACTIONINFO QUIT
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
S: * TRANSACTIONINFO 70644955-ef32-4d33-a88b-67b500a7c00d PAN 92873498723498792384
S: * TRANSACTIONINFO 70644955-ef32-4d33-a88b-67b500a7c00d ApprovalCode 643972
S: * TRANSACTIONINFO 70644955-ef32-4d33-a88b-67b500a7c00d Mileage 176439
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
