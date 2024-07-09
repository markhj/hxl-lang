# Expression evaluation

> **Important**:
> This is a draft of an HXL feature which is not yet part of the official specification

## Overview

Capability to evaluate simple expressions.

## Motivation

Provides a ton of flexibility.

## Specification

### Syntax

#### Evaluation

````text
const PI: 3.1415

<NodeType> A
    key: 2 * PI
````

#### Uniform/global

Similar to GLSL (shader language) values can be "imported" from
whatever global scope is utilizing HXL.

```text
import bool IsHard

<NodeType> A
    health: IsHard ? 100 : 70 
```

#### Functions

`````text
<NodeType> A
    age: GetAge(1990)
`````

Between parsing and semantic validation, there must be another transform
layer.

Pseudo code:

````cpp
transform.func("GetName", [](std::vector<std::string> args) {

});
````

Alternatively, functions could be declared in the document.

````text
func multiply(int a, int b) -> a * b

<NodeType> A
    key: multiply(2, 5)
````

### Semantics

- Global/universal values must be defined

### Examples

N/A

## Impact

### Backward compatibility

- **Syntax in previous implementations**: The new syntax will not
  be compatible with interpreters translating older specifications.

- **Parsing older sources**: Implementations of this new standard
  can parse older specifications.

### Performance

Depending on the expression complexity, the number of expressions
and the implementation, there could be some impact on performance.

## Open questions

### Breaking of language purpose?

HXL is a data interchange format. Adding actual logic and
expression evaluation _might_ break the decoupling between
data and domain.

But looking at why HXL was invented, it could make sense.
It was invented to aid a game engine in setting up a scene.
Having at least some degree of expression evaluation provides
a great deal of flexibility and abstraction.

### Optional feature

It could be leverage the implementation to make expression evaluation
optional to the end-user.
