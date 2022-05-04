## Expressions

Expressions are how the world allows users to define interactivity. They allow the user to
define small chunks of logic that can then be executed.

### `ExpressionData` vs. `Expression`

Expressions are defined using `ExpressionData` which is defined using protobuf and
can be serialized and sent to other players and stored in the database.
The `Expression` class defines the expression in C# and provides the functionality.
The `ToEpxressionTool` allows the conversion from `ExpressionData` to `Expression`.

Finally, you can make the expression an executable function with the help of `Expression.Compile`.
This happens automatically when the world is played.

### Scope

Expressions get executed in a `Scope`. The scope defines the variables that an expression can access.
Each object has its own scope, and there is also a global scope.

The scope contains:
- A set of default objects, like `gameObject` which allows the user to access things like the object's name
- All behaviours on the object, for instance allowing the user to change `Physics.weight`.
- Any custom variables the user defined for the object.

Scopes works hierarchically, meaning that each scope contains all variables that are present in the
parent. For example, if you create a car object and give it a `passenger` , then any part of
that car can access `passenger`.

### Expression Types

There are a few types of expressions that can be held by an ExpressionData instance. Each type
of expression has a field in the ExpressionData (this slightly weird way of doing inheritance
is the best approach to make the data work with protobuf).

#### AssignExpressionData

`AssignExpressionData` represents the setting of a value to another value, e.g. `x = 5`.
The `Right` field must be the new value, and the `Left` field must be some editable value,
either a PropertyExpression or a VariableExpression

#### BinaryExpressionData

`BinaryExpressionData` represent some basic operation with two arguments, e.g. `2+2`.
The `Operator` is a `System.Linq.Expressions.ExpressionType` cast to an integer so that
it can be serialized by protobuf.

#### ConstantExpressionData

`ConstantExpressionData` is simply a literal value, e.g. `5` or `"Zaphod Beeblebrox"`.
It is a ValueData which can hold most Unity types as well as simple things like numbers and strings.

#### MethodExpressionData
`MethodExpressionData` represents calling a method of an object, e.g. `Motion.Turn(90, 1)`. The 
`MethodName` is the name of the method you want to call, and the `Source` should be the object that
has that method. Common values for `source` are variables like `gameObject` or `Motion` that 
contain a lot of useful methods.

The `ReturnType` is an (currently missing) enum representing the type of return value.

To pass arguments to the method call, you can add to `Arguments` field.

#### BlockExpressionData
`BlockExpressionData` is a set of expressions. This is useful when wanting to do multiple things
in a single event response.

#### PropertyExpressionData
`PropertyExpressionData` represents accessing the property of some value, e.g. `Physics.weight`.

#### UnaryExpressionData
`UnaryExpressionData` is an expression with an operator and a single argument, e.g. `!Motion.isMoving`.
The only two unary operators implemented are the boolean negate (`!`) and the numeric negative (`-`).

#### VariableExpressionData
`VariableExpressionData` allows you to access variables in the scope that the expression is being
executed in. `SourceObject` can be used to get variables from some arbitrary object, but otherwise
the current scope is searched.

### Prebuilt Expressions

There are a set of prebuilt expressions in `OpenUp.Updating.Helpers.EventUpdateHelper.PrebuiltConditions`
and `OpenUp.Updating.Helpers.EventUpdateHelper.PrebuiltResponses`. These can serve as
guide on how to construct other similar expressions.