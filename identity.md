# Federated but decentrailzed identity management

Existing federated identity systems often depend, nevertheless, on
some centralized server.

## Backend, RESTful API

Any particular server accepts LTI, potentially other OAuth-based
authentication.

## Frontend, client-side JavaScript

The frontend code attempts to log in on page load, and displays the
currently logged-in user in the upper-right corner (along with a
progress bar, a logout button, and social media logins for open
identity servers).  Optionally, a page may also include certain
elements with special HTML ids.

The institution is displayed along with their shortname, and perhaps
even a coursename if the LTI connection came from a specific course
context.

Logging out means that the identity token is logged out everywhere.

### id.login()

Perhaps we already have a session, so we use that.

Otherwise, generate an unstable, but hopefully globally unique browser
fingerprint (if fingerprinting fails, we can fallback to a rely on a
cookie at a central server).  Register this browser fingerprint with
any known identity server, which performs some distributed-hash-table
lookup to determine the "home" server for this user.

Use the "most recent server" to identify the server that is
authoritative for this user.