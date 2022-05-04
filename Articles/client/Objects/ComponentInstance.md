## Component Instance
This class represents an instance of an object, based on the base data in the solution.
Besides a reference to a copy of the base data, it also contains instance-specific
references. 

|Field|Discription|
||
|objectRef| The ObjectRef of the object, acting as its id.
|componentId| The unique id of this instance.
|objectInstance| The [ObjectInstace](#object-instance) of this object.
|visual| A reference to the [Visualization](../Visualizations/Visualization.md) of this instance.
|scope| A reference to the [Scope](../Events/ExpressionData/ExpressionData.md#scope) of this object.
|baseData| A reference to the [base data](../Objects/Overview.md) of the object.
|wasDestroyed| A boolean used for preventing the use of object data if an object has been removed.

|Method|Discription|
||
|GetEventNames|Returns a list of event names from this object.
|GetChildrenIDs|Returns an array of [ObjectRef](../Objects/ObjectRef.md)s for each child of this object.

### ObjectInstance
This class holds a data-equivalent of an instance of base object data. Like 
[ObjectRef](../Objects/ObjectRef.md) it holds the base data ID, the instance id, and the
id of the parent object. Additionally it holds a copy of the base data.
