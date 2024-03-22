# Additional Receipt Information (`RECEIPTINFO`)

:warning: **NOTE: This extension is discontinued, and will be removed in future versions.**

Every transaction and clearing creates many data points on the gas station site. This information is valuable to the client and the gas station for traceability in case of issue resolution. Using the `RECEIPTINFO` capability the server announces, that additional receipt info may be sent while clearing the transaction.

## `RECEIPTINFO`

Type: **Notification**

Sends additional receipt information to the server while clearing, but before the clearing ended. The information is provided using key / value pairs.

Arguments:

- **FSCTransactionID** (arg0, uuid): ID given by the server for informational reasons (defined by the server). Needs to be provided by the client for the clearing transaction id to correlate the information.
- **Key** (arg1, string): Name of the receipt info (see list below)
- **Value** (arg2, string): Value of the receipt info

List of known receipt infromation keys, additional or other keys are silently ignored:

- `Trace-No` Tracing number
- `Terminal-ID` Terminal ID
- `Receipt-No` Receipt number
- `Site-ID` Site ID as defined by the mineral oil company
- `Local-Date` Date of the receipt in the local date and format of the site
- `Local-Time` Time of the receipt in the local time and format of the site
- `VU-No` Related contract number

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
