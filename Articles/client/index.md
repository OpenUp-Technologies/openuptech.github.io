# Client Core Library

This article is the home page for everything to do with the client core library.

The client core library allows the running and editing of solutions built using the OpenUp platform.

### Table of Contents

1. [General Overview](#general-overview)
2. [World Definition](#world-definition)
3. [Connection and Session](#connection-and-session)
4. [Edit/Play](#editplay)
5. [Updating Worlds](#updating-worlds)
6. [Objects](#objects)
7. [Behaviours](#behaviours)
8. [Events](#events)
      - [Expressions](#expressions)

### General Overview

### World Definition

Every world has a base [environment](Environments/Environments.md). A kind of 3D background with some pre-existing
objects inside. When a user designs a tutorial, in such an environment, these two form the "world". The user can
invite others to join in on the design and/or testing of the tutorial, who can give immediate feedback as they are
literally, virtually, in the same little world. Walking around and interacting with it, without needing to
physically be in the same room.

The world is therefore the environment and interactions that are experienced by the users.
Not to be confused with "Solution", which refers to the raw data of a world, usually located on the server.

### Connection and Session

### Edit/Play

Every world has two states, we call these edit- and play- mode. 

In edit mode, objects have no physics, and separate
objects can be picked up and placed, no matter their size or assigned weight. Some visual and physical properties
can be changed by selecting an object, and objects can be given [behaviours](#Behaviours). Users can also add
[events](#Events) to lose objects or edit them while the world is in this mode.

During playmode, all edits, behaviours and events take effect. Objects that are given physics will realistically
fall to the ground, thrown around, or unable to be picked up due to their assigned weight. While in playmode users
can't change any of the properties of the objects. These changes need to take place in edit mode.

### Updating Worlds

While in edit mode, changes made to a world have to be saved, but also synchronized with anyone in the same session.
To do this we send data in the form of [UpdateData](DLL/UpdateData.md) to the server which in turn makes sure
everyone in the same session receives the same change. Depending on the kind of update you want to send, the
[UpdateData](DLL/UpdateData.md) object only needs 2 or 3 fields set.

While in playmode, the same [UpdateData](DLL/UpdateData.md) objects are sent via a different method to the server.
These updates are still synchronized, but not saved. This is how we keep objects in the same position for everyone
in the session, for example.

#### Ownership

To avoid any confusion by possible updates to a single object coming from multiple people in the same session,
<b>we make sure that only the "owner" of an object can send updates</b>, while the rest only apply the updates
received from the server. All objects have a default owner assigned from the start. If someone wants to grab an
object, the code first claims ownership provided the ownership of an object is not locked.

If ownership is locked for example, because someone else is editing its properties, or has grabbed it, nobody else
can claim ownership of the object, or grab it, and edit its properties.

### [Objects](Objects/Data.md)

There's a lot of use of the word "object" with different meanings depending on the context. For more info about
the terminology, check out this [overview](Objects/Overview.md).

#### [ComponentInstance](Objects/ComponentInstance.md)

The objects in the app, defined in the data are used for creating ComponentInstances which are the unity
equivalent of the data of these objects.

#### [Hierarchy](DLL/Hierarchy.md)

Sometimes there are objects that are part of a larger object. We call these children and parent when talking about
their "relationship". For example, if we have a car-frame object, separate wheel objects would be children of that
car-frame.

### [Behaviours](Behaviours/BehaviourBase.md)
We use components collectively called Behaviors to add behaviours and interaction 
to objects. A couple of default behaviours already exist, but more can easily be added.

### Events

Users make [EventData](Events/EventData.md) for creating timed or triggered interactions/changes during playmode.
These are mostly suplimental to the behaviour components. But in theory an entire
behaviour can be set up using events.

This is done through the [EventsEditor](Events/EventsEditor.md) during playmode.

#### Expressions

The data used to compile code during playmode is called [ExpressionData](Events/ExpressionData/ExpressionData.md).
These can be generated with a limited capacity through the [EventsEditor](Events/EventsEditor.md) or set 
up programatically.