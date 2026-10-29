# Additional Receipt Information (`RECEIPTINFO`)

:warning: **NOTE: This extension is discontinued, and will be removed in future versions.**

Every transaction and clearing creates many data points at the gas station. This information is valuable to the client and the gas station for traceability when resolving issues. By announcing the `RECEIPTINFO` capability, the server indicates that additional receipt information may be sent while clearing the transaction.

## `RECEIPTINFO`

Type: **Notification**

Direction: **Client → Server**

Sends additional receipt information to the server during clearing, i.e. after receiving the `CLEAR` request and before concluding it. The information is provided as key/value pairs.

Arguments:

- **FSCTransactionID** (arg0, uuid): ID defined by the server. The client must provide the FSCTransactionID of the transaction being cleared so that the information can be correlated.
- **Key** (arg1, string): name of the receipt information (see list below)
- **Value** (arg2, string): value of the receipt information

List of known receipt information keys; any other keys are silently ignored:

- `Trace-No`: tracing number
- `Terminal-ID`: terminal ID
- `Receipt-No`: receipt number
- `Site-ID`: site ID as defined by the mineral oil company
- `Local-Date`: date of the receipt in the site's local time zone and format
- `Local-Time`: time of the receipt in the site's local time zone and format
- `VU-No`: related contract number

## Example

```
C: * CAPABILITY CLEAR HEARTBEAT LOCKPUMP PRICES PUMPS PUMPSTATUS QUIT TRANSACTIONS UNLOCKPUMP
S: * CAPABILITY BEAT CHARSET PLAINAUTH PRICE PUMP TRANSACTION LOCKEDPUMP QUIT RECEIPTINFO
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
