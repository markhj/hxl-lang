# Changes confirmed for ``HXL2025``

This document details features and changes for ``HXL2025``.
Note that at this stage features and changes have been confirmed
to be included, but they are not yet formalized. In other words,
you cannot yet trust that the syntax and behavior described are
what you'll see in the final specification.

## Features

### Enumerators

#### Suggested syntax

````text
<NodeType> A
    enum: VALUE_ONE
````

#### Rules and behavior

- Values must be written in all upper-case to be valid.
- The schema specifies enumerator values.

### Inherit properties

A node property can inherit another property (of exactly same type).
During transformation stage the node property will be physically replaced
by the referenced value.

Suggested syntax:

````text
<NodeType> A
    key: value
    
<AnotherType> B
    other: <= a.key
````

#### Rules and behavior

- Note types don't have to be identical, but properties data types do.

### Templates

Template nodes which can be inherited. They work
like regular nodes and can be inherited, but in the transformer
stage the nodes will be discarded (and thus never appear in the
deserialized structure).

Suggested syntax:

````text
<Template:NodeType> Template
    x: 20
    
<NodeType> Child <= Template
    # x is inherited
````

#### Rules and behavior

- A template node type must match the inheriting node type

### Nested properties

More complex objects require trees of properties.

````text
<Node> A
    position:
        x: 50.0
        y: 15.0
        z: -25.5
````

### Array values on separate lines

Array values can be written on multiple lines.

````text
<Node> A
    names[]:
        "John"
        "Jane"
        "Doe"
````

#### Rules and behavior

There must be one value per line.

Line breaks and empty lines are not allowed.

### Text on multiple lines

Text can be written on multiple lines.

````text
<Node> A
    text:
        This is a long text
        spread across multiple lines
````
