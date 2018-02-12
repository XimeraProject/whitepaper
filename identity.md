# Federated but decentrailzed identity management

Existing federated identity systems often depend, nevertheless, on
some centralized server.

## Backend, RESTful API

Any particular server accepts LTI, potentially other OAuth-based
authentication.

## Frontend, client-side JavaScript

### id.login()

Generate an unstable, but hopefully globally unique browser
fingerprint.  Register this browser fingerprint with a known identity
server, which performs some distributed-hash-table lookup to determine
the "home" server for this user.

Use the "most recent server" to identify the server that is
authoritative for this user.