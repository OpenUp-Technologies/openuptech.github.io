## Shape

A class that defines the properties of a certain script. Often used for 
[Behaviour](../Behaviours/BehaviourBase.md)s or [Visualization](../Visualizations/Visualization.md)s.
It is mostly a list of [PropertySpec](Shape.md) values, which define the name, type and initial value of a field on
the class this is the shape of.

The only thing you need to know about the [PropertySpec](Shape.md) is that we use it as a struct with only two 
fields. It has a `Name` field, which should be identical to the name of the property on the class this shape is
from, and a `DefaultValue`, which is a [ValueData](ValueData.md) value.

Here's an example of the process of setting one up:

Let's say we want to make a behaviour that can move an object in a given direction with a certain speed. For this
we would make a simple class with a `Vector3` property for the direction, and a `float` type property for the
speed. We'll just call the behaviour "Move".

```csharp
public class Move
{
    public Vector3 direction;
    public float speed;

    //Other logic to make an object move
}
```

The `Shape` for this behaviour will look like this:

```csharp
Shape moveShape = new Shape
{
    Name = "Move",                              //This is just the displayed name of the behaviour
    MonoBehaviour = typeof(Move),               //This is the type of the class. Required for the application to
                                                //automatically add a component of the right type.
    [1] = new Protobuf.AppData.PropertySpec     //Note that we start indexing from 1. NOT 0.
    {
        Name = nameof(Move.direction),          //The name has to match the name of the property exactly.
        DefaultValue = Vector3.forward          //The default value the property should have.
    },
    [2] = new Protobuf.AppData.PropertySpec
    {
        Name = nameof(Move.speed),
        DefaultBalue = 0
    }
}
```