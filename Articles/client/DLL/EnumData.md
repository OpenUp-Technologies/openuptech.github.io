## EnumData
This is a [ValueData](ValueData.md) type made to function a bit like an `enum`.

|Field|Description|
||
|`state`|Returns the [ValueData](ValueData.md) in the `Options` list, at the index of `SelectedIdx`|
|`Options`|A list of the possible [ValueData](ValueData.md) values this enum could have. Can only be set once.|
|`SelectedIdx`|The `int` value of the state this enum is currently in. Can be set.|

|Method|Description|
||
|`SelectState`|Returns true if the given [ValueData](ValueData.md) was found in the `Options` list, and sets its `SelectedIdx` to the index of the item in the list.|

Other than setting the `SelectedIdx` the state can also be set using the `SelectState` method.

Use example:

```csharp
EnumData CardinalDirection = new ValueData 
{
    Enum = new EnumData
    {
        SelectedIdx = 0,
        Options =
        {
            new ValueData{ Text = "North" },
            new ValueData{ Text = "East" },
            new ValueData{ Text = "South" },
            new ValueData{ Text = "West" },
        }
    }
};

Debug.Log(CardinalDirection);

CardinalDiraction.SelectedIdx = 2;
Debug.Log(CardinalDirection);

TextData west = "West";
Debug.Log(CardinalDirection.state == west);

CardinalDirection.SelectState(west);
Debug.Log($"Cardinal direction {CardinalDirection} has options index {CardinalDirection.SelectedIdx}")

```
Console will show:

>North
>
>South 
>
>false
>
>Cardinal direction West has options index 3