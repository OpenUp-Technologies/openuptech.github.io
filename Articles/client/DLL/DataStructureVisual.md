## Visual representation of world data structure

### AppRuntimeData
```json
AppRuntimeData =
{
    Objects =
    {
        [1] = (ObjectStructure),
        [2] = (ObjectStructure),
        ..
        //List of object structures with an index
    },
    GlobalVariables =
    {
        [1] = (Variable),
        [2] = (Variable),
        .. 
        //Collection of variables with an index
    },
    GlobalEvents = 
    {
        [1] = (EventData),
        [2] = (EventData),
        ..
        //Collection of events with an index
    },
    RootHierarchy = (Hierarchy)
    Environment,            //Name of the environment
}
```

See also: | [ObjectStructure](../Objects/Data.md) | [Hierarchy](../Objects/Hierarchy.md) | [EventData](../Events/EventData.md) |

### ObjectStructure
```json
{
    Id,                     //The id of this object data
    Name,                   //A default or given name to this object
    LockState,              //A flag enum for locking motion, rotation or scale on certain axis
    Transform,              //Unity transform data
    TransformCorrection,    //Unity transform data
    Visual = (Visual),
    Children = (Hierarchy),
    Behaviours = 
    {
        [1] = (Behaviour),
        [2] = (Behaviour),
        ..
        //List of behaviours added to this object
    }
}
```

See also: | [ObjectStructure](../Objects/Data.md) | [Visualization](../Visualizations/Visualization.md) | [Behaviour](../Behaviours/BehaviourBase.md) |

#### Hierarchy structure
```json
{
    [1] = (Child),
    [2] = (Child),
    ..
    //Collection of events with an index
    ChildOrder[]            //Int array representing the order to load in the children with the given indexes.
}
```

See also: | [Hierarchy](../Objects/Hierarchy.md) | [Child](../Objects/Hierarchy.md#Child) |

#### Child structure
```json
{
    ChildID,                //The ID or index of this child in a Hierarchy
    ObjectID,               //The ObjectStructure id of this object
    Override = (ObjectStructure)    //Edits made to an object are stored here and applied during playmode.
}
```

See also: | [Child](../Objects/Hierarchy.md#Child) | [ObjectStructure](../Objects/Data.md) |

#### Behaviour structure
```json
{
    Type,
    Properties = 
    {
        [1] = ValueData,
        [2] = ValueData,
        ..
        //A list of values matched with a certain index number. 
    }
}
```
The index number of the values corrispond with the number a property has in the [Shape](Shape.md)
of a behaviour of a type matching the number given in `Type`.

See also: | [ValueData](ValueData.md) | [Shape](Shape.md) |

#### Visual structure
```json
{
    Type,
    Properties = 
    {
        [1] = ValueData,
        [2] = ValueData,
        ..
        //A list of values matched with the number of a property in a Visual of the given type.
    }
}
```
The index number of the values corrispond with the number a property has in the [Shape](Shape.md)
of a Visual of a type matching the number given in `Type`.

See also: | [ValueData](ValueData.md) | [Shape](Shape.md) |

#### Variable structure
```json
{
    Name,                   //Name of the variable
    Value = (ExpressionData),
    CurrentValue = ValueData
}
```

See also: | [ValueData](ValueData.md) | [ExpressionData](../Events/ExpressionData/ExpressionData.md) |

#### EventData structure
```json
{
    Id,                     //The id of this event in an event collection
    Index,                  //The order in which this event should be played
    Name,                   //User given name to the event
    GroupName,              //Name of a group of events this is part of
    ActiveAtStart,  
    Condition = (ExpressionData),
    Responce = (ExpressionData)
}
```

See also: | [EventData](../Events/EventData.md) | [ExpressionData](../Events/ExpressionData/ExpressionData.md) |

#### ExpressionData structure
```json
{
    Value,                  //Expression
    Template,               //Name of the expression template this is based on
    CustomName,             //User given name to the expression
}
```

See also: | [ExpressionData](../Events/ExpressionData/ExpressionData.md) |