## Environments

Every world is built inside an Environment. This environment sets the overall
tone and theme of the world. Worlds built in the office environment feel different
from worlds built in a nature environment, or a empty space.

The environment's contents are defined entirely client-side allowing it to
contain objects and Unity behaviours far too complex to be defined in the world data.

### Editor Window

The environments can be edited from inside the project settings under Edit > Project Settings > OpenUp > Environments Settings
window. It lists all the environments and their properties.

#### Active/Inactive Environments
Here you will find all environments that the user can choose from under "Active Environments" when running the app.
It also lists any environments in the project that are not options for the user under
"Inactive Environments". You can activate/deactivate an environment with the
`Activate`/`Deactivate` button under the name of the environment after you unfold it in the menu editor window.

### Environments Data

From the perspective of the world data, and environment is a simple string. Turning
this string into the game objects is the responsibility of the client. Here is where we make sure the client
loads in the correct environment.

Each environment is defined using an `EnvironmentOption` scriptable object asset. These options
are in turn contained in an `EnvironmentRegistry` asset that grants the rest of the app
access to all known environments.

#### EnvironmentOption

The `EnvironmentOption` defines an environment and has the following fields

| Field | Definition |
| --- | --- |
| `environmentName` | The name of the environment. This is what is stored in the world data and as such should not be changed. |
| `rootObject` | The prefab that is the root of the environment. |
| `skybox`| An optional material used as a custom skybox. |
| `prefabPaths` | A list of paths to prefabs that fit thematically with the environment. These will be exposed to the user to add to the world. |

### [Adding Objects to the Environment](ConvertToSolution.md)

While any object in the environment prefab will also be in the world, to make the object part
of the world data so that it can be edited requires a bit more work. 

To add an object as part of the world data, the objects will be removed from the environment instance loaded
and re-added via the program. For the program to determine which objects these are, they require a [ConvertToSolution](ConvertToSolution.md)
component containing the path to the environment prefab, which should be in the `source` field of the [ConvertToSolution](ConvertToSolution.md) component.

When a world is created using an environment, the root object of the environment prefab is searched for all objects
with this component. They are converted to `UpdateData` which makes the program re-add the object and adds it to the starting data for the
world. This will essentially define the object in the world data, which will allow it to be edited from within the application.
Any changes in the environment prefab will flow over to the environment loaded in the application.