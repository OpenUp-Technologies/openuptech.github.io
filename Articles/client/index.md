# Client Core Library

This article is the home page for everything to do with the client core library.

The client core library allows the running and editing of solutions built using the OpenUp platform.

### Table of Contents

1. [General Overview](#general-overview)
2. [World Definition](#world-definition)
3. [Connection and Session](#connection-and-session)
4. [Edit/Play](#editplay)
5. [Updating Worlds](#updating-worlds)
6. [Objects](#objects)
7. [Behaviours](#behaviours)
8. [Events](#events)
      - [Expressions](#expressions)

### General Overview

### World Definition

[Environment](Environments/Environments.md)

### Connection and Session

### Edit/Play

### Updating Worlds

### Objects

[Objects](Objects/Overview.md)

[ComponentInstance](Objects/ComponentInstance.md)

[Hierarchy](Objects/Hierarchy.md)

[Overview](Objects/Overview.md)

### Behaviours
We use components collectively called [Behavior](Behaviours/BehaviourBase.md)s to add behaviours and interaction 
to objects. A couple of default behaviours already exist, but more can easily be added.

See also: | [Animate](Behaviours/Animate.md) |
[Button](Behaviours/ButtonBehaviour.md) |
[ColorTransition](Behaviours/ColorTransition.md) |
[Connector](Behaviours/Connector.md) |
[Gaze](Behaviours/Gaze.md) |
[Highlight](Behaviours/Highlight.md) |
[Motion](Behaviours/Motion.md) |
[Physics](Behaviours/Physics.md) |
[StreamSource](Behaviours/StreamSource.md) |
[TwistInteraction](Behaviours/TwistInteraction.md) |

### Events

Users make [EventData](Events/EventData.md) for creating timed or triggered interactions/changes during playmode.
These are mostly suplimental to the behaviour components. But in theory an entire
behaviour can be set up using events.

This is done through the [EventsEditor](Events/EventsEditor.md) during playmode.

#### Expressions

The data used to compile code during playmode is called [ExpressionData](Events/ExpressionData/ExpressionData.md).
These can be generated with a limited capacity through the [EventsEditor](Events/EventsEditor.md) or set 
up programatically.

See also:
| [ConstantExpression](Events/ExpressionData/ConstantExpressionData.md) |
[VariableExpression](Events/ExpressionData/VariableExpressionData.md) |
[PropertyExpression](Events/ExpressionData/PropertyExpressionData.md) |
[BinaryExpression](Events/ExpressionData/BinaryExpressionData.md) |
[AssignExpression](Events/ExpressionData/AssignExpressionData.md) |
[MethodExpression](Events/ExpressionData/MethodExpressionData.md) |
[BlockExpression](Events/ExpressionData/BlockExpressionData.md) |