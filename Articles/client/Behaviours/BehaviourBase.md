## BehaviourBase

Objects can have specialized scripting attached to them using behaviours. 
These behaviours have a list of properties and are kept in sync across multiple
users by keeping the properties in sync.

### Synchronization and Ownership

Behaviours should be run according to their ownership. Each object has exactly
one owner and that owner must match the behaviour's properties to the behaviour's
state as the app is run. All other users are listeners and they must match the 
behaviour's state to the behaviour's properties.

