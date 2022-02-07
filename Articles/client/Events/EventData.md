## Event Data

Interaction during playing a world is governed by events. An event is defined
as a condition-response pair. Each object has a set of these pairs and each
frame all conditions are checked and for each true condition, the response
is executed.

### Fields

| FieldName | Description |
| --- | --- |
| `Id` | A unique identifier for this event |
| `Index` | An integer to determine the order of the events |
| `Name` | A human-readable name for the event |
| `GroupName` | Allows grouping of events during editing, no effect during play |
| `Condition` | An [Expression](ExpressionData.md) that returns either true or false, to determine whether the event has been triggered |
| `Reponse` | An [Expression](ExpressionData.md) which describes what must be done if the event is triggered |
| `ActiveAtStart` | Whether the event should be listening when the object is loaded in |

### Events during Play

When the world is played all events are compiled, this turns their `Condition`
and `Response` into executable functions. Each frame all objects run each active
event, checking the `Condition` and if that returns true, firing the `Response`.

Because all events are compiled during the start, the events cannot be updated during
play, and multiple instances of the same base obejct cannot have different events.

#### Inactive Events

Each event can be turned inactive. When an event is inactive, its `Condition`
is not checked and thus its `Response` is never fired.
By default, events are active when an object is loaded in. To override this you
can set the `ActiveAtStart` field to `false`.

To disable an event while playing, you can use the Expressions. A method expression
to call `gameObject.SetEventActive("EventName", false)` is how that would work. The prebuilt
response `"Set Event Active"` does this.

#### Execution Order

The order in which multiple events on the same object are fired is influenced
by their `Index`. If the `Index` is not set then the event's index is `0` and
it would be executed first. The order of multiple events with the same index is
undefined.
The firing of events is single-threaded and there is no way to fire some events
of object A, then object B's events and then other events of object A.

### Events during Editing

Events are defined on the objects, and are stored in an `EventCollection` on the object's
`Events` field.

#### Changing Events
Like all world data, events can be updated by sending an `UpdateData` to the server. The
`EventUpdateHelper` class has the following extension methods to easily edit some properties.

| Method | Description |
| --- | --- |
| `EventData.SetName(string newName)` | Updates the name of the event. |
| `EventData.SetIndex(int newIndex)` | Updates the index of the event. |
| `EventData.SetCondition(ExpressionData newCondition)` | Updates the condition of the event. |
| `EventData.SetResponse(ExpressionData newResponse)` | Updates the response of the event. |
| `EventCollection.DeleteEvent(int id)` | Removes the event with that id |
| `EventCollection.AddEvent(EventData eventData = null)` | Adds a new event and returns the new event's id |

These are extension methods and therefore you must add `using OpenUp.Updating.Helpers;` to be able to
use these methods.

**NOTE:** these updates get sent to the server and then bounced to all players. They do not get applied
immediately to the world data when you send an update. This means that directly after sending
an update to an event, that event will still have its old data for a few frames before updating.