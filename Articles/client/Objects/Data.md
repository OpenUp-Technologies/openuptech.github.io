## Object Data

Every GameObject in a world is defined as an object. Both 2D and 3D models are objects,
so are groups. The object data contains not just its visual representation
but also its interactive behaviour.

### `ObjectStructure`

An object has its data stored in an `ObjectStructure` and has the following fields.

| FieldName | Description |
| --- | --- |
| `Id` | A unique identifier for this object |
| `Name` | The name of the object. |
| `LockState` | BitMask that locks the ways the user can manipulate the object. It holds separate values for Edit and Play modes. |
| `Transform` | The transformation for object, i.e. its position, rotation, and scale. This is relative to the parent. |
| `TransformCorrection` | Fixes any importing errors in the object's transform, should usually be identity |
| `Visual` | The visual representation of the object, usually a model or text. Can be nothing (for example with groups). |
| `Children` | The children of the object are stored in here. |
| `Behaviours` | Any specialized scripting components are listed here. |
| `Events` | [Events](../Events/EventData.md) are stored in this field. |
| `LoalVariables` | Custom variables for events are stored here. |

### Visuals

What you actually see of an object is called the object's 'visual'. The most common
visuals are the Model, Text, and Primitive visuals. Each visual has its own
list of properties it can have, and those properties define what the object looks like.

### Groups

A group is simply a object without a visual, it behaves the same way and has
all the same properties. It can have events associated with it, have behaviours,
and have custom variables defined.

There is even no requirement for a group to have any children, though this is by
far the most common use-case. Also, there is currently no way in the edit-mode UI
to select an object without a collider (this makes selecting an empty group 
rather difficult).

### `PropertyList` and `Shape`

The properties of both Behaviours and Visuals are stored in a `PropertyList`.
This stores a set of values in key-value mapping with each property having an
integer ID associated with it.

Which properties can be in the list is set via a `Shape`. This shape holds the
ID, name, and default value for each field in the list. It also holds a integer id
for the entire shape and a human-readable name for the list, as well as reference
to the MonoBehaviour that is added to display the visual/run the behaviour.

For example, the shape for the Text visual is: 

```c#
new Shape
{
    Name = nameof(Text),
    MonoBehaviourClass = typeof(Text),
    [1] = new PropertySpec
          {
              Name = nameof(text),
              DefaultValue = "Text"
          },
    [2] = new PropertySpec
          {
              Name = nameof(color),
              DefaultValue = Color.black
          },
     // cut for brevity
}
```

This allows you to access the properties of the Text either by name, or
by id.

```c#
// Get the text using the field's ID
string text = properties.GetProperty(1);

// Get the text using the field's name
text = properties.GetProperty("text");
```

### [Hierarchy](Hierarchy.md)

The internal structure of the hierarchy is flat, all objects are stored in one 
list at the same depth. The children of an object are not represented as all the
data for that object but as a reference using the child object's ID.

When the same object is loaded in multiple times, it can be given slightly different
properties using the child's `Overrides` field. Use this to, for example, spawn in
the same tree ten times at different positions.

The hierarchy only defines which objects should be children when the object is loaded
in, during playing other objects can be loaded in by events or behaviours.

### `ObjectInstance` vs. `ObjectStructure`

When the app is played, each time an GameObject is loaded in a copy of the object is
made and that copy is what you see loaded in. This copy has a unique instance ID.
During play, events and behaviours should edit this object instance, not the base data.

In short: `ObjectStructure` is the base data for an object, `ObjectInstance` is a
specific instance of that object that has been loaded in.