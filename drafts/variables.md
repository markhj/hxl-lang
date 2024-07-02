# Draft: Variables

> **Important**:
> This is a draft of an HXL feature which is not yet part of the official specification

## Variables

Must be declared at the beginning of the file (but after file includes).

Variables only have effect in the file in which they take part.

## Declaration suggestions

````text
a: 45
````

````text
var a: 45
````

## Usage suggestions

````text
<NodeType> A
    key: *a
````

````text
<NodeType> A
    key: &[a]
````
