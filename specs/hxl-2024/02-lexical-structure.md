# Lexical structure

This chapter provides a helicopter-view of the language features. In the following chapter
they will be reviewed and analyzed in-depth.

## Character set

HXL implementations must support **Unicode** to provide
cross-platform and cross-language flexibility  in the data.

## Basic syntax

An HXL source consists of one or more **nodes**.
Every node consists of zero, one or more **properties**.

An example from a game engine would be:

````text
<Player> MainCharacter
    name: "John Doe"
    health: 100
````

- ``<Player>`` is the **node type**. This is a required declaration, which eases deserialization
  when the data is interpreted.
- ``MainCharacter`` is the **node name**.
- ``name`` and ``health`` are **node properties**. ``name`` is a string, ``health`` is an integer.

Every node must have a type and name. They can, theoretically, have any number
of properties, although they must satisfy a schema.

## Data types

The data types listed below are currently supported.
Please see the chapter on data types for more in-depth review.

- Strings
- Booleans
- Integers
- Floating-point values
- Arrays
- References

## Arrays

A node property can contain a set of values, using arrays. Arrays are
defined by suffixing ``[]`` to the property name.

````text
<Player> MainCharacter
    position[]: { 5, 0, -5 }
````

## References

A node property can reference another node, by suffixing ``&``:

````text
<Player> MainCharacter
    health: 100
    
<Enemy> Monster1
    target&: MainCharacter
````

In this case, ``Monster1``'s ``target`` property references ``MainCharacter``.

## Inheritance

Inheritance means a node will copy all the properties from another,
and override the properties explicitly defined.

Example:

````text
<Enemy> MonsterOne
    health: 100
    position[]: { 4, 0, 4 }
    
<Enemy> MonsterTwo <= MonsterOne
    position[]: { 8, 0 , 8 }
````

In this example, ``MonsterTwo`` implicitly copies ``health`` (as 100).

## Comments

Any text following ``#`` on any line should be ignored, with the only
exception being when the ``#`` is within a string literal.

````text
# Define the first enemy
<Enemy> MonsterOne
    health: 100
    position[]: { 4, 0, 4 } # Monster starts close to player
````

## Drafts

See the ``drafts`` directory to explore features currently being outlined.
