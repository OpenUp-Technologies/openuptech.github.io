# Objects

### Terminology

Worlds in Aryzon World are built up out of objects. The term 'object' is however
rather ambiguous here as it can refer to anything from the 3D model to Unity's 
`GameObject` to the data that defines the object, to even the basic programming concept 
from Object Oriented Programming. 

To that end, let's define some terms.

| Term | Definition |
| --- | --- |
| In-game Object | This refers to a GameObject in Unity that is part of the World. |
| Model | This refers to a 3D visual, consisting of triangles and vertices. |
| Base Data | This is the data that defines in-game objects, represented using a `ObjectStructure`. |
| Instance Data | This is the  data for a single in-game object, represented using a `ObjectInstance`. |
| Client Instance | This data structure holds all the client logic for an `ObjectInstance`. Represented using a `ComponentInstance` |
| Object | This is a catch-all term we use that is a combination of "in-game object", "instance data" and depending on the context "base data".|

The base data and instance data are shared across all users. In each client the client 
instances are driven from this shared data and and those client instances then create and 
manage the in-game objects and models.

### Object Data

[Main Article](Data.md)

Each in-game object has exactly one instance data associated with it. This instance data
defines it properties and interactions. The instance is based of some base data and that
base data is what is stored in the world data.

### Object Hierarchy

[Main Article](Hierarchy.md)

Objects are organized into a hierarchy when the world is played. In the world data
all object are at the same depth though. Each object data has a list of IDs of
the objects that it should load in as children. In-game, these objects will then be
children.

