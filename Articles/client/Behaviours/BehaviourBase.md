## BehaviourBase

> This page is partially obsolete and will be updated in the near future

Objects can have specialized scripting attached to them using behaviours. 
These behaviours have a list of properties and are kept in sync across multiple
users by keeping the properties in sync.

It's important to not that properties and methods you want the user to be able to use through expressions need
to be marked with the [OpenToUsers](../Utils/OpenToUsersAttribute.md) attribute.

The `BehaviourBase` is an abstract class so this will remain untouched when making behaviours, but there are a 
couple of things to note.

|Field|Description|
|---|---|
|`type`|An abstract enum representing the numerical identifier of the behaviour.|
|`elementID`|A.K.A. [ObjectRef](../Objects/ObjectRef.md) The id of the object in the environment.|
|`data`|The [ComponentInstance](../Objects/ComponentInstance.md) holding the same data the object has in the server.|

|Method|Description|
|---|---|
|`GetPropsShape`|A virtual method that returns a [Shape](../DLL/Shape.md) that you do **have** to override if you want the behaviour to hold any data on the server.|
|`GetProperty`|Returns the [ValueData](../DLL/ValueData.md) of the property with the given id or field name.|
|`SetProperty`|Sets the property of the given id or field name, to the given [ValueData](../DLL/ValueData.md)|
|`OnDataUpdate`|A virtual method, used for updating the behaviour if the data has been updated through the server.|

### Synchronization and Ownership

Behaviours should be run according to their ownership. Each object has exactly
one owner and that owner must match the behaviour's properties to the behaviour's
state as the app is run. All other users are listeners and they must match the 
behaviour's state to the behaviour's properties.

Note: 
- Ownership may change during the application.
- An object will always have an owner.

___

### Existing Behaviours
Here are some quick introductions of the existing behaviours: 

#### Animate
This behaviour is for adding animation components to the object and exposing the animation names to be invoked
via the events. It also has some properties to change how the animations run (fast, slow, looping, etc.)

#### Button
A behaviour that simply exposes whether or not it has been pressed. This can be used in events as a condition to
trigger responces.

#### ColorTransition
This behaviour can be used to color something to a certain gradient between `colorA` and `colorB`. You can also
have the coloring happen over a given amount of time. This way you can have a steel object color bright red over
time to simulate it heating up.

#### Connector
An unfinished behaviour mainly for connecting objects as though they are connected by joints.

#### Gaze
Another simple behaviour that exposes if an object is being looked at. This can be used in events as a condition
to trigger responces.

#### Highlight
This behaviour gives objects a glowing outline for if you need to draw a users attention to something.

#### Motion
This behaviour features many possibilities for moving objects. Including the ability to set up paths for an object
to follow. Or targets to follow. `speed`, `direction`, `duration`, `targets`, `loop`, `isColliding` are to 
name but a few of the properties one can use to simulate certain motion behaviours.

#### Physics
The Physics behaviour is deceptively important. It's what makes objects move and collide as though they are real 
objects. It has but a few properties for weight, drag and whether or not to use gravity.

#### StreamSource
Not a behaviour that is used often, or at all. But something we would use to spawn objects like particles. As of
writing, we don't use the particle components of unity. This streamsource is not supposed to spawn too many objects.
Just single objects to suggest a stream of some sort.

#### TwistInteraction
A behaviour used about as much as the StreamSource, but much more complicated as it allows for objects to represent
certain values depending on how far it's rotated. Imagine it to work like a valve in combination with the 
StreamSource. The more the object is rotated, the more objects are spawned from the StreamSource.
But it could also be used for a wheel to steer a car.

