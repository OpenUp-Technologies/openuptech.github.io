## Environments

Every world is built inside an Environment. This environment sets the overall
tone and theme of the world, worlds built in the office environment feel different
from worlds built in a nature environment, or a empty space.

The environment's contents are defined entirely client-side allowing it to
contain objects and Unity behaviours far too complex to be defined in the world data.

### Editor Window

The environments can be edited from inside the project settings in the `OpenUp/Environment Settings`
window. It lists all the environments and their properties and allows you to edit them.

#### Active/Inactive Environments
It lists all environments that the user can choose from under "Active Environments".
It also lists any environments in the project that are not options for the user under
"Inactive Environments". You can activate/deactivate an environment with the
`Activate`/`Deactivate` button.

### Environments Data

From the perspective of the world data, and environment is a simple string. Turning
this string into the game objects is the responsibility of the client.

Each environment is defined using an `EnvironmentOption` scriptable object. These options
are in turn contained in an `EnvironmentRegistry` that grants the rest of the app
access to all known environments.

#### EnvironmentOption

The `EnvironmentOption` defines an environment and has the following fields

| Field | Definition |
| --- | --- |
| `environmentName` | The name of the environment. This is what is stored in the world data and as such should not be changed. |
| `rootObject` | The prefab that is the root of the environment. |
| `skybox`| An optional material that defines a custom skybox. |
| `prefabPaths` | A list of paths to prefabs that fit thematically with the environment. These will be exposed to the user to add to the world. |

### [Adding Objects to the Environment](ConvertToSolution.md)

While any object in the environment prefab will also be in the world, to make the object part
of the world data so that it can be edited requires a bit more work. 

To add an object as part of the world data, the object must be removed from the environment
and added via the program upon creating the world. to do this, add the `ConvertToSolution`
component to the object and set the prefab root path to a prefab containing the object.

When a world is created in this environment, the root object is searched an all objects
with this component are converted to `UpdateData` and added to the starting data for the
world. This allows them to be edited and synchronized.