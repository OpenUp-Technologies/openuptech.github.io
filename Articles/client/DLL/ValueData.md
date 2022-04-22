## ValueData

This class represents primitive and enumerable values that are saved or sent through the server. As you'll see, 
the type names are self explanatory.

|Name|Description|
||
|BoolData| Returns: `bool` (true, false)|
|NumberData| Returns: `float` (But can be assigned an `int` or `double`)|
|TextData| Returns: `string` |
|Vector3Data| Returns Unity type: `Vector3` but has properties for `x`,`y` and `z`|
|ColorData| Returns Unity type: `Color` but has properties for `r`,`g`,`b` and `a`. (Red, Green, Blue, Alpha respectively) Can also be used as a `Vector4`.|
|ListData|Returns: `List<ValueData>` |
|DictData|Not fully implemented, but should function like a `Dictionary<ValueData><ValueData>`|

Special types:

|Name|Description|
||
|[EnumData](EnumData.md)|A special value type, designed to work like an `enum` except it can be more than just an int. [Click](EnumData.md) for more details.|
|ModelData|Contains an url or directory under `url`|
|TransformData|Contains a `TransformStructure` for its value, which in turn has three values for `position`(Vector3), `rotation`(Quaternion) and `scale`(Vector3)|

### Usage

Except for the special types, ValueData types are made to implicitely convert themselves into the unity value equivalents. So you should
be able to assign and compare these with unity primitive values without issue. For example, the following code
sample gives no compiler errors and works perfectly fine:

```csharp
NumberData num = 1;
if(num > 0) Debug.Log($"{num} is bigger than zero.");

BoolData testBoolean = false;
if(!testBoolean) Debug.Log($"testBoolean is {testBoolean}.");
```

When run, the console will show:
>1 is bigger than zero.
>
>testBoolean is false.

ValueData itself can also be used as a **var** type, to a certain extent. So `ValueData num = 1;` works just
fine. But there might be issues using it as such. Because the `ValueData.Type` might still be Null.
It's a bit safer to define it as:
```csharp
ValueData num = new ValueData { Number = 1 };
```

It's still advised to use the specific ValueData types as much as possible. Especially if they are never
supposed to have a different value type.

Here are some examples for initializing ModelData and TransformData types.
(See [EnumData](EnumData.md) for examples of its use.)

```csharp
ModelData model = new ModelData { Url: "https://something.so" };

TransformData trans = new TransformData
{
    Value = new TransformStructure
    {
        position = Vector3.zero,
        rotation = Quaternion.identity,
        scale    = Vector3.one
    }
}
```
