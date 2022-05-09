## Object Hierarchy

In-game objects in a world support parent-child behaviour. The in-game objects loaded in at the start are
defined using a set of `Child`ren. These children do not contain the full data of
the object to be loaded in, just the object's ID.

### Child

Each `Child` object has the following fields.

| FieldName | Description |
| --- | --- |
| `ChildID` | A unique identifier for this child. |
| `ObjectID` | The ID of the object that should be loaded in. |
| `Overrides` | This field allows you to customize the loaded object specifically for this child. |

An object loaded in by a child can be customized using the `Overrides` field. This field
is also an `ObjectStructure` and is recursively merged on top of the base data.

#### During Edit mode

When the editor allows you to change an object that has been loaded in, you must
consider whether you want to change the base object, or the `Overrides`.

Each loaded in object has a `sourceChild` field that references the `Child` that loaded
it in. This is part of the [ComponentInstance](../Objects/ComponentInstance.md).

This direct relation between `Child` and instance is only present during edit mode.
When the world is played children are loaded in on a fire-and-forget basis. An object
is loaded in, it loads in its children and then those children are on their own.

### `Hierarchy`

Children are stored in a `Hierarchy`. The hierarchy has a key-value map holding all the children
by ID as well as a separate list representing the order for the children. The reason
the children are not just a list is to avoid multi-user editing errors when the children
are reordered while a change to a child is being sent.

#### Root Hierarchy

Note that objects in the object data are not loaded in automatically, they must be referenced
as a child somewhere or loaded in via an event or behaviour. Each world has a root hierarchy 
which defines which objects should be loaded in on start. When you add a new object, a `Child`
is usually also made and added to the root hierarchy. 

### Synchronization

Synchronization of the hierarchy is extremely important to make sure the world looks the
same for all users. This is handled by the `HierarchyLoader` and the `RootObject` components.
Each in-game object in Unity has a `HierarchyLoader` attached to it and that loads in that in-game object's Hierarchy. 
The `RootObject` loads in the root hierarchy.

#### Sync in Edit mode

During edit mode each in-game object attempts to load or destroy child in-game objects to match its object's hierarchy data. 

#### Sync in Play mode

During play mode, when an in-game object is loaded in, each child gets added as `ObjectInstance`
using the `Child` data. Child in-game objects are then loaded in or removed to match the collection
of all `ObjectInstance`. Object instances have a parent instance id associated with them
for this purpose.

*Note:* Finding the children for each parent is done by the `RootObject` in a single
loop, not each `HierarchyLoader` for itself. This reduces the amount of times the
`ObjectInstance` collection gets looped over.