# Federated but decentralized identity management

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

## Key concepts

### No dependencies

The identity management layer works independent of any other components of this system and is as generic as possible so that it can function in many contexts.  It provides links to interact with other layers, if they are present.

### Communicating identity via browser fingerprint or session cookie

User establishes an identity (i.e., some token) via logging in through any institution that supports this protocol.  At that point, the identity becomes associated with a browser fingerprint (or a cookie?) and that association is broadcasted to the network.  Then, when the user accesses a page that employs the identity layer, that page can retrieve the identity from the network based on the browser fingerprint.  At that point, the page can set a session cookie so that the identity is maintained even if the browser fingerprint changes.  If a cookie is already present, the page must still check with the network to determine if the session that maps to the identity is still active (i.e., the user hasn't logged out).  If the session is still active, then the page will use that identity.  Otherwise, the page will attempt to determine the identity from the browser fingerprint.

What about if two different computers have the same fingerprint?  No guarantee of uniqueness.  Would need to favor diversity of fingerprint over stability.

Should there also be a way for a course site to communicate the identity token directly to the page via link parameters?  This could avoid needing to use browser fingerprinting.

### Determine versioning information based on instructor setting

Once the identity has been established, the identity layer obtains any versioning information established by the instructor.  (The mechanism by which the instructor specifies this information will distribute this information across the network to ensure the lookup is speedy and doesn't require a connection to any computer at the instructor's institution.)  For example, the instructor may have specified that all students see the same version or that each student get their own randomly selected version.

### Specify and identify version of page

The identity layer passes sufficient information to the page to specify the version of the page.  It receives back from the page information to uniquely identity the page version (so that, for example, one can identify which users received the same version even in case of the randomly selected versions).

The simplest scenario is that the identity layer passes an integer version number to the page, and the page employs a many-to-one map from that integer to a specific page version.  The page then passes back to the identity layer via a one-to-one mapping a canonical version number of that page.  (The canonical version number can be used as a version number to recreate the exact page version.)

More complex scenarios could involve any json object is lieu of the integer which could be used, for example, to control the version of specific page components.  The identity layer is agnostic as to any meaning of the json version indicated.  As in the integer case, the assumption is the presence of a many-to-one mapping from the json object to a specific page version and that the page pass back a canonical json object that identifies the page version.

### No identifying information

The identity layer does not pass any information that could be used to identify the user.  In addition, it does not pass any information that could be used to infer enrollment in any course.  Instead, each user will be identified by an anonymized token.

To display to the user readable information about how they are logged in, the identity layer can receive a handle both for the user and for course enrollment.  To prevent leaking of information, these handles should be initialized to values that bear no relationship to the user or course (such as random strings).  A user could edit those strings as they desire, as long as they are informed that these handles will be communicated to outside entities.

### Behavior if no identity found

If a user accesses a page and it cannot determine user's identity, then page can decide what version to display.  Page could also be configured to display different options for users to log in.  Could display social media logins.  Could give users an option of logging in via any institution that has linked to the page.  (Not sure how page would have this information.)

### Information display

The identity layer would display information about who is logged in.  It should display the handles for user and course, if they exist.  It should also supply hooks for the display of a progress bar, course navigation (e.g., previous/next links), and other information from the course instructor (such due dates or course structure).

This information should be displayed in a default location (e.g., upper right corner) unless the page author specified a different location via a particular id on a DOM element.

### Hooks to other layers

The identity layer initiates the connections to the appropriate locations for communication by the other layers.  It will be responsible for routing data such as progress/grade data, event stream data (xAPI or Caliper), and page state data.  It will determine the proper connections from the federated network using the identity token.

If the user wanted a local copy of this information, it would presumably go through the identity layer.
