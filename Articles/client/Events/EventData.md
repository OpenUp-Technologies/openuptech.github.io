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
| `Condition` | An [Expression](ExpressionData/ExpressionData.md) that returns either true or false, to determine whether the event has been triggered |
| `Reponse` | An [Expression](ExpressionData/ExpressionData.md) which describes what must be done if the event is triggered |
| `ActiveAtStart` | Whether the event should be listening when the object is loaded in |

### Events during Play

All events are compiled once the world is played, this turns their `Condition`
and `Response` into executable functions. Each frame all objects run each active
event, checking the `Condition` and if that returns true, firing the `Response`.

Because all events are compiled during the start, the events cannot be updated during
play.

#### Inactive Events

Each event can be turned inactive. When an event is inactive, its `Condition`
is not checked and thus its `Response` is never fired.
By default, events are active when an object is loaded in. To override this you
can set the `ActiveAtStart` field to `false`.

To disable an event while playing, you can use the Expressions. There's a prebuilt responce called
`"Set Event Active"` which is compiled into the method that sets an event active or inactive. You just need
to fill in the event name and whether or not you want it to be active.

You can also make this expression programatically. To get started, you can read up about it at [ExpressionData](ExpressionData/ExpressionData.md).

#### Execution Order

The order in which multiple events on the same object are fired is influenced
by their `Index`. If the `Index` is not set then the event's index is `0` and
it would be executed first. Though events can have the same index, the order of multiple events with the same 
index is undefined. So if order is important, make sure events don't share the same index.

The firing of events is single-threaded. For example, assuming object A was added first, if events in object B 
cause events in object A to have their conditions fulfilled, those events in object A will have to wait for the
next go-round.

So you will have to work around situations where a condition relies on a state of an earlier object that can only 
be set on the next loop. For example:

```csharp
ObjectA
{
    Event1
    {
        Condition: //If ObjectA is moving..
        Responce: //Set variable 'isMoving' to true.
    }
    Event2
    {
        Condition: //If ObjectA reached a certain point..
        Responce: //Set variable 'isMoving' to false.
    }  
}
ObjectB
{
    Event1
    {
        Condition: //On start..
        Responce: //Move ObjectA.
    }
    Event2
    {
        Condition: //If ObjectA.isMoving is false..
        Responce: //Destroy ObjectA.
    }
}
```
In this example, ObjectA is not moving when it starts, so it's "isMoving" variable is false. Events 1 and 2 do
nothing. The first event in ObjectB starts moving ObjectA immediately. Its second event then checks if the variable 
"isMoving" on ObjectA is true. But **only on the next loop** will ObjectA respond and set its "isMoving" variable
to true. So ObjectA will be destroyed before it even got to its destination.

All of these events could have been placed in ObjectA in the same order, and the same would happen. But in this
example we choose to show an ObjectB influencing another object for you to **keep in mind that things can get very
complicated, and the results confusing, if you don't keep track of what's happening.**

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