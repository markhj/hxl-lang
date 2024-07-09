# New data types

> **Important**:
> This is a draft of an HXL feature which is not yet part of the official specification

## Overview

List of ideas for new data types.

## Motivation

Provides more flexibility.

## Specification

### Bitflags

````text
<NodeType> A
    bitflags: 00101101
````

#### Alternative for clarity

````text
<NodeType> A
    bitflags:
        opt_a: true
        opt_d: true
````

Or:

````text
flag opt_a: 16
flag opt_d: 32

<NodeType> A
    bitflags: opt_a | opt_d
````

#### Semantics

- Reads right-to-left
- Values not provided in the flag will be interpreted as ``0``
- Exactly 8 bits (1 byte)

### Maps

````text
<NodeType> A
    meta[#]:
        a: 10
        name: "John"
        b: true 
````

The contents of a map will not be evaluated against the schema.

### Tri-state values (-1, 0, 1)

No idea for syntax yet.

### NULL

Explicitly set a value to null (regardless of type).

````text
<NodeType> A
    key: null
````

> Note: It's very questionable that this will ever be implemented,
> as non-required values can just be omitted for the same effect.

### Range

````text
<NodeType> A
    key[..]: { 5, 10 } 
````

#### With steps

````text
<NodeType> A
    key[..]: { 0, 15, 5 } 
````

## Impact

### Backward compatibility

- **Syntax in previous implementations**: The new syntax will not
  be compatible with interpreters translating older specifications.

- **Parsing older sources**: Implementations of this new standard
  can parse older specifications.

### Performance

There should be limited impact on performance.

## Open questions

N/A
