## UpdateData

Any UpdateData class holds information regarding a change to the solution data. So when initializing one, it needs
to be given some data. These are usually 2 to 3 values depending on the kind of update you want. Here we will just
name the most important and/or frequent used.

|Field|Description|
||
|Type|The type of update this is. Usually "Set", "Composite" or "Delete"|
|Path|An [UpdatePath](#UpdatePath) path, or directory to reach the specific point in the data you want to edit.|
|Updates|A list of UpdataData for sending an update that consists of a multitude of updates.|
|NewValueData|A [ValueData](ValueData.md) value which is the base class of then OpenUpTech equivalents for unity or primitive types.|
|NewExpression|A new [Expression](../Events/ExpressionData/ExpressionData.md) you want to assign. Usually to an event.|

Besides the two "New..." properties, there are also fields for specific primitive types. Their names speak for
themselves, such as "NewValueString", "NewValueBool", "NewValueInt" and "NewValueUInt".

#### Set
If the update you want to send, sets or edits a value, the type you give it is obviously `UpdateType.Set` .
This kind of update needs two other fields set. One of them is the `Path`. This is an UpdatePath that points to
where in the entire solution data, the change should be made.
Finally you also need to give it a value ofcourse.

In edit mode, if you give a path that does not exist, the fields will be created and have the given value assigned.
So if we pretend we have some completely empty solution data like: `{ //empty }`, and we give the following 
update:
```csharp
UpdateData myUpdate = new UpdateData
{
    Type = UpdateType.Set,
    Path = {"Objects", "MyObject", "Name"},
    NewValueData = new ValueData{ Text = "Sunny" }
}
```

The solution data that first looked like: `{  }`, if presented as JSON data will now look like this:
```json
{
    "Objects":
    {
        "MyObject":
        {
            "Name": "Sunny"
        }
    }
}
```

#### Delete
An update to delete, remove or clear something in the solution data is one of "Type" `UpdateType.Delete` . These
updates just need one other field set, the `Path`. This update will clear the entire piece of data the path points
to. For example, lets get a JSON representation of some example solution data:

```json
{
    "Objects":
    {
        "MyObject":
        {
            "ID": //some id ,
            "Name": "MyObject",
            //Properties
        }
    }
}
```

A delete-type update with a path that represents the path to `Name` (which would read like: `Objects.MyObject.Name`)
Would remove the "Name" field entirely. This won't cause any trouble, but it's important to realize. The solution
data only has fields that have values. It's how we can save space. But the code still understands that an object
has a "Name" field, and will return an empty string. Where an attempt to read it from the raw solution data
directly would result in `null`.

#### Composit
Composit updates only need to be assigned the type `UpdateType.Composit` and given a list of `UpdateData`.
These updates will be applied in order.

### UpdatePath

Back when our code was still very inefficient, the data on the server was literally json data. So paths to specific
fields would literally be a list of strings. But this took up so much space, updates slowed the program down.
We have since started using binary forms of data and while an `UpdatePath` can still consist of a list of strings
it's adviced to use a list of numbers instead.

Luckily most objects already hold a reverence to the path they're on already. Including a constant number for their
field. [ObjectRef](../Objects/ObjectRef.md) itself also has a method called `ObjectPath` containing the path to the 
data of the object with that id.
An `UpdatePath` can also easily be "extended" by simply using the `&` operator. So here's an example of creating a
path to an event on an object, assuming we know the `ObjectRef` called "objRef" of an object:
```csharp
UpdatePath = objRef.ObjectPath() & ObjectStructure.EventsFieldNumber & eventID;
```

It does require some prior knowledge about the [DataStructure](DataStructure.md) to know that there's an "EventsFieldNumber" that has
to be added before the id of an event.


