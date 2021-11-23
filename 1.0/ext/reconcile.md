# Transaction reconciliation (`RECONCILE`)

During an `UNLOCKPUMP` initiated process disconnects or other error events may lead to an unfinished pre-authorization on the server side.

This extenstion enables the server to automatically reconcile on the status of the transaction. The server will be able to actively request transactions using the `RECONCILE` command.

In case the `PUSHING` method is used the server already gets transactions pushed after the login. Otherwise the server
would ask for `TRANSACTIONS` after the site connected. The problem is, that if the site doesn't remember the transaction due to a day change or similar, the transaction maybe left open on the server indefinitely. The `RECONCILE` will therefore be issued after a connect if there is no
data for a specific `TRANSACTION` presented to the server.

The site has to respond with either a `TRANSACTION` or no data in case the transaction is not known or was otherwise not processed by the system.

## `RECONCILE`

Type: **Request/Response**

Request to reconcile the transaction identified by the transaction id.

Arguments:

- **FSCTransactionID** (arg0, uuid): the transaction id that was used by the server to `UNLOCKPUMP`.

Errors:

- **404** Transaction unknown and was definitely not handled by the system

## Example

```
C: * CAPABILITY CLEAR HEARTBEAT LOCKPUMP PRICES PUMPS PUMPSTATUS QUIT TRANSACTIONS UNLOCKPUMP TRANSACTIONINFO RECONCILE
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
C: * PUMP 2 locked
C: * PUMP 3 locked
C: * PUMP 4 locked
C: S1 OK
S: S2 UNLOCKPUMP 1 EUR 100.0 70644955-ef32-4d33-a88b-67b500a7c00d pace
```

**!! Network disconnected !!**

### Reconnected next day (transaction was processed)

This example shows that the transaction was indeed processed and that the money needs to be captured and payed to the MOC.

```
C: * CAPABILITY CLEAR HEARTBEAT LOCKPUMP PRICES PUMPS PUMPSTATUS QUIT TRANSACTIONS UNLOCKPUMP TRANSACTIONINFO RECONCILE
S: * CAPABILITY BEAT CHARSET PLAINAUTH PRICE PUMP LOCKEDPUMP TRANSACTION TRANSACTIONINFO QUIT
C: C0 CHARSET ISO-8859-1
S: C0 OK
C: C1 PLAINAUTH 9eb56d5e-6563-430a-9d39-5ddf567e73d5 1d3b755d3bce8f09b4f8ff08dabf1796
S: C1 OK
S: S1 TRANSACTIONS
C: S1 OK
S: S2 RECONCILE 70644955-ef32-4d33-a88b-67b500a7c00d
C: * TRANSACTION 3 70644955-ef32-4d33-a88b-67b500a7c00d open 0100 EUR 86.83 72.978 19.0 13.65 LTR 54.40
C: S2 OK
...
```

### Reconnected next day (transaction not processed)

This example shows that the transaction was not processed and that no money need to be payed to the MOC.

```
C: * CAPABILITY CLEAR HEARTBEAT LOCKPUMP PRICES PUMPS PUMPSTATUS QUIT TRANSACTIONS UNLOCKPUMP TRANSACTIONINFO RECONCILE
S: * CAPABILITY BEAT CHARSET PLAINAUTH PRICE PUMP LOCKEDPUMP TRANSACTION TRANSACTIONINFO QUIT
C: C0 CHARSET ISO-8859-1
S: C0 OK
C: C1 PLAINAUTH 9eb56d5e-6563-430a-9d39-5ddf567e73d5 1d3b755d3bce8f09b4f8ff08dabf1796
S: C1 OK
S: S1 TRANSACTIONS
C: S1 OK
S: S2 RECONCILE 70644955-ef32-4d33-a88b-67b500a7c00d
C: S2 ERR 404 transaction unknown
...
```

