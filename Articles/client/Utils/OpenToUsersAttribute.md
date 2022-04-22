## OpenToUsers-Attribute
This attribute is what we use to expose certain properties or methods on scripts to be reached through events.
Events are the only way users can change most properties, or invoke behavioural methods. So this attribute is
absolutely crucial for us to allow users to do that.

There are a couple of different constructors fur this attribute. The more parameters you provide the more nuance
there is to when the property/method is exposed to users.

|Field|Definition|
|---|---|
|`displayName`|The name of the property or method that the user will see.|
|`complexityLevel`|Intended for future personalized tutorials to strategically hide properties/methods that are too complicated to expose from the start. Default value is 1.|
|`phase`|An enum describing the state the property is in. This is for example to be able to make a property usable in events, but if not fully tested yet, not show up in the build. Default value is STABLE.|
|`defaultToolTip`|A tooltip we can show the users if they hover over this option. Default value is empty.|

This Attribute should only be used on properties that are of the type `ValueData` or methods with returntype void
or `ValueData`.

Use example:
```csharp
[OpenToUsers("Speed", 1, Phase.STABLE)]
ValueData speed { get; set; }
```