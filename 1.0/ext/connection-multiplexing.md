# Connection Multiplexing (`NEWSESSION`, `SESSIONS` and `SESSION`)

To allow multiplexing connections on a single communication channel we introduce an extension to the `<tag>` and a special method called `NEWSESSION`. A multiplexed connection is called "session" and is identified by a prefix in the `<tag>`. This prefix has the form `<sessionID>.<tag>`. To create such a session the client first opens a communication channel to the FSC server and starts the handshake process. To create a new multiplexed connection the client issues a `NEWSESSION` message without authenticating the initial connection. If the connection is closed (e.g. with `QUIT`) all its sessions will also be closed. Besides that each session can be closed independently with `QUIT`. If the client authenticates (e.g. with `PLAINAUTH`) before issuing a `NEWSESSION` message, no further `NEWSESSION` calls are allowed. In this case the initial connection (without session prefix) will be the only connection on the specific communication channel.

## `NEWSESSION`

Type: **Request/Response**

Direction: **Client → Server**

Creates a new session with the provided **SessionID**. If the call was successful, OK will be returned. In case of an error the server responds with an ERR message. In case of success, all subsequent messages belonging to the new session need to prepend a session prefix to their tag. The new tag will be sent in form `<sessionID>.<tag>`. If `<sessionID>` is missing (only `<tag>`), the message will be assigned to the initial session/connection. **Priority** marks the importance of a session for a site. If a site connects through multiple sessions, the server will only communicate, with the site, through the session with the highest priority (1 is highest priority). If this session closes or fails, the server will switch to the session with the next highest priority. If no priority is specified the server will automatically set the priority based on the order in which the site connects.

Arguments:

- **SessionID** (arg0, session): identifier of the created session.
- **Priority** (arg1, number, optional): priority of the session. Must not be 0 if specified.

Errors:

- **409** SessionID already taken
- **410** NEWSESSION after PLAINAUTH

  If this extension is used, the `PLAINAUTH` method will return two additional error codes. The first one if `PLAINAUTH` is called on the initial connection after a `NEWSESSION` call and the second one if the defined **Priority** on the current session is already taken for the now authenticated **SiteAccessKey**:

- **409** Priority already taken

- **410** PLAINAUTH after NEWSESSION

## `SESSION`

Type: **Notification**

Direction: **Server → Client**

Information about an open session on a connection.

Arguments:

- **SessionID** (arg0, session): identifier of the session.
- **Priority** (arg1, number): priority of the session.
- **SiteAccessKey** (arg2, uuid, optional): identification of the session access key.

## `SESSIONS`

Type: **Request/Response**

Direction: **Client → Server**

Lists all currently open sessions on the current connection. This method can only be called on non session connections (no session prefix). The server needs to make sure that all sessions have been transmitted with SESSION notifications before he can send an OK to conclude the SESSIONS request. In case of an error the server returns an ERR message.

Arguments: **None**

Errors:

- **410** SESSIONS called on session (tag with session prefix)

## Example

```
C: * CAPABILITY CLEAR HEARTBEAT LOCKPUMP PRICES PUMPS PUMPSTATUS QUIT TRANSACTIONS UNLOCKPUMP
S: * CAPABILITY BEAT CHARSET LISTSESSIONS NEWSESSION PLAINAUTH PRICE PUMP LOCKEDPUMP TRANSACTION QUIT
C: C1 NEWSESSION s1
S: C1 OK
C: s1.C1 PLAINAUTH 9eb56d5e-6563-430a-9d39-5ddf567e73d5 1d3b755d3bce8f09b4f8ff08dabf1796
S: s1.C1 OK
S: s1.S1 PUMPS
C: C2 NEWSESSION s1
S: C2 ERR 400 sessionID 's1' already in use
C: s1.* PUMP 1 free
C: s1.* PUMP 2 free
C: s1.* PUMP 3 in-use
C: s1.* PUMP 4 ready-to-pay
C: s1.S1 OK
C: C3 NEWSESSION s2
S: C3 OK
C: s2.C1 PLAINAUTH 9eb56d5e-6563-430a-9d39-5ddf567e73d5 1d3b755d3bce8f09b4f8ff08dabf1796
S: s2.C1 OK
C: C4 PLAINAUTH 9eb56d5e-6563-430a-9d39-5ddf567e73d5 1d3b755d3bce8f09b4f8ff08dabf1796
S: C4 ERR 410 plainauth after newsession not allowed
C: C5 NEWSESSION 1337
S: C5 OK
C: C6 SESSIONS
S: * SESSION s1 1 9eb56d5e-6563-430a-9d39-5ddf567e73d5
S: * SESSION s2 2 9eb56d5e-6563-430a-9d39-5ddf567e73d5
S: * SESSION 1337 1
S: C6 OK
```

## EBNF

```bash
session_char = char - "." .
session = session_char { session_char } .

tag = [ session "." ] ( "*" | ident ) .

newsession_method = "NEWSESSION"
	space session
	[ space number ] .

session_method = "SESSION"
	space session
	space number
	[ space uuid ] .

sessions_method = "SESSIONS" .
```
