## Implementation notes

### Schema

Implementations should provide a schema which outlines:

- Supported node types
- Supported properties on a per node type basis
- The data type of each property
- Whether a data type is required
- (Optional) Default values for properties

How the schema is defined and implemented can largely be done
at the implementors own discretion.

### Pipeline

The recommended pipeline is:

- Tokenization
- Grammar validation
- Pre-schema validation
- Schema validation
- Translate to language-specific objects

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

In the **grammar validation** you verify the order the tokens are provided
in. Building on the example above, we would expect an identifier to follow
the ``<`` delimiter. If that isn't the case, we encounter "an unexpected token."

In the **pre-schema validation**, you verify structures prior to using the
schema. This, for example, verifying that an array doesn't have mixed types
or verifying that references exist.

In the **schema validation** stage, you hold the structure up
against the schema. Here you verify that node types are correct, that
required properties exist, that unrecognized properties are caught, etc.

In the final stage, **translation to language-specific objects**,
the results are provided in objects which are readable by the implementation
language. This could be structs, classes or something else.

For C++, it could be a struct, in PHP an array with nested associative arrays.
This step might be more difficult in language which have limited reflection
capabilities.

### Other notes

It's recommended to design your implementation such that you support
multiple specifications, and the client can select which one
they adhere to.

It's also recommended that implementations which are to be used across
different projects, such as generalized libraries for HXL, provide a
way to dynamically select which features to allow. Not all of HXL's features
are fit for every kind of project.

Node types, node names and properties are case-sensitive.