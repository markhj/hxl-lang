# File system features

> **Important**:
> This is a draft of an HXL feature which is not yet part of the official specification

## Overview

This document discusses three features:

- Include HXL file
- Stream file content into property
- Reference a file

## Motivation

Separating content into files _can_ increase organization of
the data structure, and make a HXL file more readable.

## Specification

### Syntax

#### Include file

````text
include "another.hxl"
````

#### Stream

````text
<Shader> MyShader
    vertex<<: "vertex-shader.glsl"
    fragment<<: "fragment-shader.glsl"
````

#### Reference file

When referencing the file is not loaded into the nodes (by the
HXL implementation), but a struct/class must be produced by
the deserializer, which references the file

````text
<DataSheet> MyBudget
    csv: "my-budget.csv"
````

> Note: The schema will be responsible for interpreting this
> as a reference to a file.

### Semantics

- Files must exist

### Examples

N/A

## Impact

### Backward compatibility

- **Syntax in previous implementations**: The new syntax will not
  be compatible with interpreters translating older specifications.

- **Parsing older sources**: Implementations of this new standard
  can parse older specifications.

### Performance

The feature implementation in itself should not have noticeable
performance implications.

However, the usage of especially file streaming, can open usage
of the language up to performance and memory issues.

## Open questions

- Should absolute paths be allowed? Presumably, not suitable for
  exchanging data across platforms.
