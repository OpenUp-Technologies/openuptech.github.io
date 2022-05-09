## DataStructure
Though a big binary chunk of data in the database, world data still follows a structure for updates to take place at
specific locations in that data. This pace is directed towards programmers who want to send [UpdateData](UpdateData.md)
for some automatic edits to the data when a user adds something to their objects, or make other edits to an object
while in edit mode.

Scroll to the bottom for a quick visual of the data structure.

#### AppRuntimeData

Starting from the top, the world data has a couple of fields in it:

|Field|Description|
||
|Objects|A list of all the lose objects as [ObjectStructure](../Objects/Data.md#ObjectStructure)s that are in this world.|
|GlobalVariables|A list of variables that might be important for events, and are accessible from anywhere.|
|GlobalEvents|A list of [events](../Events/EventData.md) are not connected to an object, so they will always run in this world.|
|RootHierarchy|This is not actually a complete [Hierarchy](Hierarchy.md) of the world. These are just the root objects in the world. Objects with no parent.|
|Environment|The name of the [environment](../Environments/Environments.md) this world runs in.|
|Buffers|Used for holding chunks of data to easily edit the world without having to override big parts of the world over and over.|

Here's a complete overview of the AppRuntimeData structure.
![](https://github.com/OpenUpTech/openuptech.github.io/tree/main/assets/images/DataStructureVisualized.jpg)