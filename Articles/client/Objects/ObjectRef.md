## ObjectRef
This is an object representing the unique id of an object. It contains a reference to the id of it's
parent and children, if it has any. While having a unique number id of its own.

Here are the most relevant aspects for programmers:

|Field|Description|
||
|`parentId`|The `ObjectRef` of the parent|
|`children`|A read-only collection of the `ObjectRef`s of the children.|

|Method|Description|
||
|IsParentOf|Returns true if this object is the parent of the object with the given `ObjectRef` with an additional parameter for if you just care about the immediate child, or a child farther down the hierarchy.|
|IsChildOf|Returns true if this object is the child, or "grand-"child of the object with the given `ObjectRef`.|