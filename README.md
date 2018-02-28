# Ximera Whitepaper

The goal of this whitepaper is to describe small, reuseable pieces
that facilitate people in learning mathematics online.

## Content

Authors must have confidence that their hard work will last.  To
achieve that level of sustainability necesstitates relying on
time-tested formats (like TeX, XML, or other plaintext markup formats)
and eschewing platforms tied too tightly to content presentation.
This separation of content from its deployment has additional benefits
in terms of accessibility.

## Deployment

The pieces include:

* [`id.js`](./identity.md) for determining student [identities](./identity.md).
* [`db.js`](./page-state.md) for storing a student's [page state](./page-state.md) (locally or at their own institution).
* `lrs.js` for logging events (at their own institution and with the page author) and grade data.
* `cafs.js` for publishing content which updates when instructors make changes.
* [`interactive.js`](./interactive.md) for managing reuseable [javascript widgets](./interactive.md).

These pieces are designed so that, regardless of where db.js is served
from, the web page can reach out to the student's institution to store
page data, and caching it in local storage (indexed db) until it can
be stored with their institution.

This means that a web site, served statically, could simultaneously
report grades and page state back to a student's institution while
also recording statistics for the author.  An instructor could "assign
a URL" and then see how his/her students perform on the assignment at
that URL.
