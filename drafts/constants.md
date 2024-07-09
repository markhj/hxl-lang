# Constants

> **Important**:
> This is a draft of an HXL feature which is not yet part of the official specification

## Overview

Declare constant values (outside of node scope), which can be used
in node properties.

## Motivation

Makes it easier to manage values across the document, which are
supposed to be identical.

## Specification

### Syntax

#### Declaration

````text
const CONST_VAL: 50
````

#### Usage

`````text
<NodeType> A
    key: *CONST_VAL
`````

### Semantics

- Must be uppercase
- Underscore allowed (as long as it's not first or last)
- Must be declared prior to usage
- Must match data type with property where it is used

### Examples

````
const ENEMY_HEALTH: 100

<Enemy> MonsterOne
    health: *ENEMY_HEALTH
    pos[]: { 3, 0, -8 }
    
<Enemy> MonsterTwo
    health: *ENEMY_HEALTH
    pos[]: { 11, 2, 20 }
````

## Impact

### Backward compatibility

- **Syntax in previous implementations**: The new syntax will not
  be compatible with interpreters translating older specifications.

- **Parsing older sources**: Implementations of this new standard
  can parse older specifications.

### Performance

When correctly structured there should be no performance impact.

## Alternatives

- Template/abstract nodes (also in draft)
- From the end-user's side, properties can, in some cases, be omitted
  and instead managed with deserialization handles.

## Open questions

- Should data type be explicitly declared, for instance ``const<int> A: 10``?
