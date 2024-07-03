# Implementation notes

It's ultimately you who're at the driver's seat of the implementation.
But this page contains some suggestions and things to keep in mind, when
implementing an HXL parser/interpreter.

## Schema

Implementations should provide a schema which outlines:

- Supported node types
- Supported properties on a per node type basis
- The data type of each property
- Whether a data type is required or optional
- (Optional) Default values for properties

How the schema is defined and implemented can largely be done
at the implementor's own discretion.

## Pipeline

The recommended pipeline is:

- Tokenization
- Parsing
- Semantic validation
- Deserialization
- Schema validation

Please note that this pipeline is a **suggestion**, and that ultimately
you decide how to implement HXL.

> Schema validation could be done prior to deserialization, but it's likely
> easier and better decoupling to do it after.

### Tokenization

**Tokenization** is the process of scanning the file, essentially by moving
a cursor over every character. During this process, you build tens, hundreds or
thousands of tokens.

Example, in the following:

`````text
<NodeType> NodeName <= ParentNode
`````

It could be tokenized as:

| Block          | Token type |
|----------------|------------|
| ``<``          | Delimiter  |
| ``NodeType``   | Identifier |
| ``>``          | Delimiter  |
| `` ``          | Whitespace |
| ``NodeName``   | Identifier |
| `` ``          | Whitespace |
| ``<=``         | Delimiter  |
| `` ``          | Whitespace |
| ``ParentNode`` | Identifier |
| ``\n``         | New-line   |

It's recommended to tokenize whitespace characters such as space,
new-line and tabulation, because it's relevant for validating
a number of syntax rules.

### Parsing

The parser primarily carries two responsibilities:

- Verify the grammar (i.e. the order tokens are provided in)
- Sort the tokens into a hierarchical tree structure, which can be consumed
  by the deserializer.

The parser is responsible for some validation, but still at a
somewhat "local" and primitive state. It can raise errors, for instance
when a set of tokens are in an illogical order. But should _not_ 
verify that referenced nodes exist, for instance.

### Semantic validation

The next recommended phase is **semantic validation**.

In this phase, we traverse the parse tree, and perform "higher level"
validation.

Among other things, we want to check that array values are of identical
types, and that referenced nodes exist.

### Deserialization

In the **deserialization** stage, you translate the parser tree to
language-specific structures. This could be structs, classes or something else.
The best choice of structure depends on language and sometimes use-case.

#### C++ example

````text
<NodeType> HelloWorld
    age: 45
````

````cpp
struct NodeType {
    int age;
}
````

### Schema validation

In the **schema validation** stage, you hold the deserialized structures up
against the schema. Here, you verify that node types are correct, that
required properties exist, that unrecognized properties are caught, etc.

## Forward- and backward-compatible specifications

It's recommended to design your implementation such that you support
multiple specifications, and the client can select which one
they adhere to.

## Language settings

It's also recommended that implementations which are to be used across
different projects, such as generalized libraries for HXL, provide a
way to dynamically select which features to allow. Not all of HXL's features
are fit for every kind of project.

## Case

Node types, node names and properties are case-sensitive.

## Annotations with rule identifiers

It's highly recommend to annotate your implementations and tests with
the corresponding rule identifier. For example:

````cpp
// Implementing GEN.001
if (source.empty()) {
    
}
````
