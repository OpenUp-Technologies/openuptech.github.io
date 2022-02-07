## Unity Specific Styling

This part of the style guide explains various style choices that are
specific to how the Unity Engine is used.

### Generics

Developers should use the generic versions of Unity methods when possible.
This means using `gameObject.GetComponent<MonoType>()` instead of 
`gameObject.GetComponent(typeof(MonoType))`. This includes calls to:
* `GameObject.GetComponent` and friends (i.e. `GameObject.GetComponentInChildren`,
`GameObject.GetComponents`, ect.).
* `Resources.Load`.
* `Object.Instatiate`

##### Reasoning

This has the benefit of being more type-safe meaning that if types are incorrect the
compiler is more likely to pick up any errors. This is particularly useful when
refactoring changes the types in your code.

For example:

```c#
public class Thing : Monobehaviour 
{
    // refactor made the field otherThing a rigid body
    // private Collider otherThing;
    private RigidBody otherThing;

    // Forgot to refactor this start call.
    private void Start()
    {
        // This does not give an error at compile time (but will break in runtime)
        otherThing = (Rigidbody)GetComponent(typeof(Collider));
        
        // This does.
        otherThing = GetComponent<Collider>();
    } 
}
```