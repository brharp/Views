# Views

## View structures

Views have a parent, a class, a name (id), text, size, position. Create a view:

create_view (class_name, view_name, style, x, y, width, height, parent, menu, session, params)

## View Procedures

Every view class has a view procedure that defines the behaviour of that class of views.  Views respond to messages.

### Example view messages

- create
- render
- show / hide
- input
- destroy

## View order

Views have a z-order. Only one top level view is displayed at a time. Requests can trigger a change in the z-order, 
which will result in a new view being rendered in response to a request.

## Child views

Child views implement controls such as buttons, text fields, etc.

## Request processing

When requests are received, request parameters (view names) are used to look up views and post input messages
to the view with the parameter value as an argument. Views respond to these messages by updating their internal 
state, triggering actions, etc.

For example, a text field will response to an input message by changing its text value. A button will response to an
input message by posting a command message to its parent view.

## Rendering

When a thread that has windows that need to be rendered (because they are new, or have been invalidated) 
polls the request queue and the request queue is empty, a render message is returned for the invalid views.
This causes the views to render content. By default a view renders all child views, and wraps the
output in a div with class="class_name".

## Sessions

Sessions are implemented as threads. As requests are received they are inspected for a session. If there is 
no active session for a request, a new session (and thread) are created. Each session has its own request/message queue.
Requests for existing sessions are added to the session request queue. Threads remove requests from their queue, dispatch
the request in their message loop to the appropriate view, and generate output. Output is sent back to the client.
