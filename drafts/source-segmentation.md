# Source segmentation

> **Important**:
> This is a draft of an HXL feature which is not yet part of the official specification

## Overview

Markers/indicators in a HXL source group nodes. In very large files
this can be used in conjunction with a file cursor, to only read out
required parts of a HXL source.

## Motivation

Improve parsing performance of large sources.

## Specification

### Syntax

````text
group A
group B

<NodeType> One
    groups[]: { A }
    
<NodeType> Two
    groups[]: { A, B }
    
<NodeType> Three
    groups[]: { B }
````

When group A is requested nodes ``One`` and ``Two`` are read from the
source, where-as requesting group B gives ``Two`` and ``Three``.

### Semantics

- Groups must exist
- Groups must be declared in the beginning of the file

### Implementation

Will most likely end up depending on some type of caching system.

- Cache is cleared when the file has an ``update`` timestamp, which is older
  than the latest cache.
- The cache will be responsible for saving the exact file cursor positions.

### Examples

N/A

## Impact

### Backward compatibility

- **Syntax in previous implementations**: The new syntax will not
  be compatible with interpreters translating older specifications.

- **Parsing older sources**: Implementations of this new standard
  can parse older specifications.

### Performance

If anything, this should increase performance. But an incorrect
implementation can, of course, have the opposite effect.

## Open questions

- Can nodes be unordered (i.e. spread throughout the entire)
  document, or should there be a requirement that they are sorted in order
  of segment?
