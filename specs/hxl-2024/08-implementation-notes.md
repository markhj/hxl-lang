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
at the implementors own discretion.

## Pipeline

The recommended pipeline is:

- Tokenization
- Grammar validation
- Pre-schema validation
- Schema validation
- Deserialization (Translate to language-specific objects)

Please note that this pipeline is a **suggestion**, and that ultimately
you decide how to implement HXL.

### Tokenization

**Tokenization** is the process of scanning the file, essentially by moving
a cursor over every character. During this process, you build tens, hundreds or
thousands of tokens.

Example, in the following:

`````text
<NodeType> NodeName <= ParentNode
`````

It could be tokenized as:

- ``<`` (Delimiter)
- ``NodeType`` (Identifier)
- ``>`` (Delimiter)
- ``NodeName`` (Identifier)
- ``<=`` (Delimiter)
- ``ParentNode`` (Identifier)

### Grammar validation

Some grammar validation can be done in the tokenization phase, but it's
recommended to fit as much of it as possible into a second layer, where
"check the grammar".

In the **grammar validation** you verify the order the tokens are provided
in. Building on the example above, we would expect an identifier to follow
the ``<`` delimiter. If that isn't the case, we encounter "an unexpected token."

### Semantic validation

In the **semantic validation**, you verify structures prior to using the
schema. This, among other things, include checking if node references
exist, if their names are unique, etc.

### Schema validation

In the **schema validation** stage, you hold the structure up
against the schema. Here you verify that node types are correct, that
required properties exist, that unrecognized properties are caught, etc.

### Deserialization

In the final stage, **deserialization**, you translate the data to
language-specific structures. This could be structs, classes or something else.

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

And then producing structs with the populated data.

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
