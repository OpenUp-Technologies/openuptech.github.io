## Events Editor

### Basic Overview
The events editor allows you to edit the events on an object. It is a
prefab that is instantiated for each object. It uses an instance of
`EventsEditorController` to read the data and receive changes to the data,
and `EventUpdateHelper` to send changes to the server.

### Configuration
The editor should be added to the project as a prefab in the project settings
under `OpenUp/UI Components/Events Editor`. The root off that prefab must contain
the `EventsEditorController` component.

### `EventsEditorController`
The `EventsEditorController` exposed the event data of an object, and a set of
Unity events for when that data changes.

| Field | Description |
| --- | --- |
| `events` | A copy of the events of an object. *Do not* set the fields directly, as that does not update the events. |
| `onActivate` | Fired when the events editor is shown. |
| `onEventAdd` | Fired when an event is added, passes the id of the added event. |
| `onEventUpdate` | Fired when an event is updated, passes the id of the updated event. |
| `onEventDelete` | Fired when an event is removed, passes the id of the removed event. |

### [Event Data Structures](EventData.md)

The events are defined as `Condition`-`Response` pairs. They also have a name and id to identify
them (The users use the name, the program uses the id).

The events can be updated by using `UpdateData`s and sending them via `Updater.SendUpdate`.
The class `EventUpdateHelper` has some extension methods to make updating events easier.

### [Expressions](ExpressionData/ExpressionData.md)

The `Condition` and `Response` of an event are both expressions. These datastructures represent
small chunks of logic that can be compiled and executed in the object's context.