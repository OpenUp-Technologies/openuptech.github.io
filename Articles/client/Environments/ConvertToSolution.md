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
| `lockState` | The lock state of the object, to for instance make an object unscalable or immovable. |
| `noPhysics` | Turns of physics for the object. |
| `weight` | The weight of the object, assuming it has physics |

There are also 2 fields to define the hierarchy.

| Field | Definition |
| --- | --- |
| `childOfRoot` | If true, then this object will be spawned in as a child of the root hierarchy |
| `children` | The list of objects that are children of this object. |

Currently, these fields must be set manually. If an object is neither `childOfRoot` nor
contained in some `children` collection, then it will not be spawned in and not part of
the world. If and object *is* `childOfRoot` *and* in some `children` collection, it will
be spawned in multiple times.

### Adding Behaviours

To add a Behaviour to a startup object, use the `IConvetToBehaviour` interface.
This interface has two members

| Field | Definition |
| --- | --- |
| `BehaviourId` | This property should return the id of the behaviour you are adding. |
| `ConvertToBehaviour` | This should return the Behaviour data. |

`ConvertToSolution` will search the object it is on for any components with this interface,
and call `ConvertToBehaviour` to add that behaviour to the object.

### Pitfalls

There are a few pitfalls to remember when using the ConvertToSolution component. 

- The object reference as `source` wil be loaded in as child of the world object.

This is due to how the objects are loaded, the object is generated and that generated object
loads in the prefab. This means that you should be careful adding things like a `Rigidbody` to the prefab as this will
result in the object moving without the world object realizing, breaking synchronization.

This also has consequences for any component or custom behaviour you added. Any behaviour
you defined will be added to the world object, not the prefab. In general the design
pattern you want to maintain is to create a Behaviour that connects the object to the
world data, and anything `MonoBehvaior` containing the custom object logic.

- Objects must be *either* `childOfRoot` *or* contained in one other `children` array.

This makes sure there is exactly one place in the hierarchy where this object must be spawned.
If this is not the case you end up with multiple instances if both are true, or no
instances if neither are true.

- The prefab's position must be centered, as the prefab's transform in the scene and the original
transform are *both* applied.

This is linked to the first pitfall. The transform in the scene is applied to the world object but the 
prefab's transform is not cleared after being loaded in, so the result is both being applied to the object.