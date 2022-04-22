## Adding editable objects to an Environment

To make an object in the environment editable, you can use the component `ConvertToSoluiton`.
By default, this component allows objects to be defined as a prefab model with or without 
physics. It also allows the user to define the objects as part of a hierarchy.

It also contains an interface that can be added to the same component to add any arbitrary
Behaviour.

### `ConvertToSolution`

To define an object to be added to the world data, you can add the `ConvertToSolution`
component. The basic object properties are defined with the following fields

| Field | Definition |
| --- | --- |
| `source` | This is the path to the prefab for this object, the `Get Prefab Path` button should configure this automatically. |
| `lockState` | The lock state of the object, to make an object unscalable or immobile, for instance. |
| `noPhysics` | Turns off physics for the object if true. |
| `weight` | The weight of the object. Does nothing if `noPhysics` is enabled. |

There are also 2 fields to define the hierarchy.

| Field | Definition |
| --- | --- |
| `childOfRoot` | If true, then this object will be spawned as a child of the root hierarchy. a.k.a. Parentless/As a root parent. |
| `children` | The list of object id's that are children of this object. |

Currently, these fields must be set manually. If an object is neither `childOfRoot` nor
contained in some `children` collection, then it will not be loaded in and not part of
the world. If and object *is* `childOfRoot` *and* in some `children` collection, it will
be spawned in multiple times as the ID is that of the object data in the solution. Not that of an individual instance of the object.

### Adding Behaviours

To add a [Behaviour](../Behaviours/BehaviourBase.md) to a startup object, use the `IConvertToBehaviour` interface.
This interface has two members

| Field | Definition |
| --- | --- |
| `BehaviourId` | This property should return the id of the behaviour you are adding. Make sure each ID is unique. |
| `ConvertToBehaviour` | This should return the Behaviour data. |

`ConvertToSolution` will search the object it is on for any components with this interface,
and call `ConvertToBehaviour` to add that behaviour to the object.

### Pitfalls

There are a few pitfalls to remember when using the ConvertToSolution component. 

- The object referenced in `source` wil be loaded in as the child of an object (Henseforth "world object") containing all the behaviour components.

This means that you should be careful adding things like a `Rigidbody` to the prefab as this will
result in the object moving without the application realizing, breaking synchronization. To put it another way, 
it will fall out of the invisible box that is the world object containing the behaviour scripts.
If the object is supposed to have a `Rigidbody` , you just leave `noPhysics` turned off. Our program will ensure a `Rigidbody` is added 
to the world object as well as any behaviour components it's supposed to have once it gets loaded in.

- Objects must be *either* `childOfRoot` *or* contained in the `children` array of another object, or they will never show up.

If the id shows up in multiple `children` arrays, or if the object is also a `childOfRoot`, there will be duplicates of the object and may result
in odd and unexpected behaviour if the object is affected by any outside variables, as both will share their identifier. You should therefore instead 
duplicate the object in the environment / solution if clones are intended.

- The prefab's position must be centered, as the prefab's transform in the scene and the original
transform are *both* applied.

This is linked to the first pitfall. Since the prefab is loaded in, as-is, and a child of a new world object. Meaning any offsets 
in the root transform of the prefab are retained.