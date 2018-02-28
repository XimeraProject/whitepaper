# Database object that captures page state

Creates a database object (db) that captures the state of all dynamic objects on the page.  All changes to the page state should be mediated through db so that the page state is always stored.   The db object will facilitate (two-way) communication between page objects and synchronize different instances of the page.

## Function as a standalone or with other components

The page state layer should work even without any other project components.  In this case, the db object should be saved to IndexedDB on the client, allowing the client to save progress on the page.  When coupled with other project components, such as the identity layer, db will be saved on a server, such as at the user's home institution.  In this case, db can be used to synchronize the page state between a student and instructor, for example.

## Paradigm for use of db

When a page loads, check if there is a db object already for the page.  In that case, the saved page state from db will be restored.  Otherwise, the page will be set to a default state.

Changes to page components should be tied directly to db so that the state is stored and other components can react by listening to change events from db.  If practical, changes should be made to db rather than page objects directly, with the page objects being changed in response to db changes.
