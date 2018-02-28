# Interactive widgets that communicate with page

Essentially a module loader for Javascript widgets that exposes the page state db object, as well as hooks for page progress (i.e., grading) and events tracking (i.e., xAPI or Caliper).

If interaction with the widget is mediated by the db object, the state of the widget will be saved and the widget can communicate with other objects on the page.

As the interactive layer requires only on the page state layer, interactives can run on stand-alone web pages or when connected to an institution.

The library should be designed to make it easy for people to copy widgets to their own web pages.
