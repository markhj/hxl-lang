# Error codes

When implementing HXL, you should provide these error codes to make sure
implementations are streamlined.

## List of error codes

Note that the **error code** is a _suggested_ number to assign to an error code, if needed.
It is **not** related to the rule identifiers (e.g. ``GEN.001`` or ``NODE.106``).

| Error code                          | Error code | Description                                                                                                                                                       |
|-------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ``HXL_EMPTY``                       | ``100``    | The HXL source is empty.                                                                                                                                          |
| ``HXL_INVALID_EOF``                 | ``101``    | An HXL file must end with a empty line.                                                                                                                           |
| ``HXL_UNEXPECTED_TOKEN``            | ``105``    | When invalid characters are found in the initial tokenization.                                                                                                    |
| ``HXL_UNEXPECTED_TERMINATION``      | ``106``    | When a new-line ``\n`` comes at an unexpected time.                                                                                                               |
| ``HXL_SYNTAX_ERROR``                | ``107``    | Failure to identify expression during grammar validation.                                                                                                         |
| ``HXL_ILLEGAL_WHITESPACE``          | ``110``    | Wrong type of whitespace, or disallowed omission.                                                                                                                 |
| ``HXL_INVALID_NODE_FORM``           | ``130``    | A node is not specified on the correct form with a type and a name.                                                                                               |
| ``HXL_INVALID_PROPERTY_FORM``       | ``131``    | A line where a property is expected, does not include a ``:``                                                                                                     |
| ``HXL_EMPTY_PROPERTY_VALUE``        | ``132``    | A node property must have a value.                                                                                                                                |
| ``HXL_ORPHAN_PROPERTY``             | ``133``    | A node property is declared outside of a node context.                                                                                                            |
| ``HXL_ILLEGAL_COMMENT``             | ``140``    | Comment is incorrectly formed.                                                                                                                                    |
| ``HXL_ARRAY_MIXED_TYPES``           | ``200``    | Mixed data types in an array. Example: ``{ "Hello", 2, 3 }``                                                                                                      |
| ``HXL_ARRAY_UNKNOWN_TYPE``          | ``201``    | When anything other than string, integer or floating-point values are found in an array                                                                           |
| ``HXL_NODE_REFERENCE_NOT_FOUND``    | ``230``    | When a referenced node hasn't been defined at an earlier point in the HXL file.                                                                                   |
| ``HXL_CIRCULAR_NODE_REFERENCE``     | ``231``    | When node A references B, which then references A. This is automatically prevented if properly implemented that a referenced node MUST exist earlier in the file. |
| ``HXL_INHERIT_DIFF_TYPES``          | ``250``    | The inherited node is of a different type than the child node.                                                                                                    |
| ``HXL_ILLEGAL_INHERITANCE``         | ``251``    | When a node attempts to inherit itself.                                                                                                                           |
| ``HXL_ILLEGAL_REFERENCE``           | ``252``    | When a node property attempts to reference the node itself.                                                                                                       |
| ``HXL_INVALID_NODE_TYPE``           | ``300``    | The node type in ``<NodeType>`` is doesn't adhere to the regular expression.                                                                                      |
| ``HXL_INVALID_NODE_NAME``           | ``301``    | The node name doesn't adhere to the corresponding regular expression.                                                                                             |
| ``HXL_INVALID_PROPERTY_KEY``        | ``302``    | The property key doesn't adhere to the regular expression.                                                                                                        |
| ``HXL_ILLEGAL_FLOAT``               | ``400``    | The format of the floating-point value is not allowed (most likely given as integer)                                                                              |
| ``HXL_ILLEGAL_STRING``              | ``420``    | The string contains illegal characters, such as ``\n``.                                                                                                           |
| ``HXL_NON_UNIQUE_NODE``             | ``500``    | When a node name is used 2+ times.                                                                                                                                |
| ``HXL_NON_UNIQUE_PROPERTY``         | ``510``    | When a node contains 2+ properties with identical keys.                                                                                                           |
| ``HXL_UNKNOWN_NODE_TYPE``           | ``800``    | The node type is not found in the schema.                                                                                                                         |
| ``HXL_ILLEGAL_DATA_TYPE``           | ``830``    | Schema expects one data type, but another is provided.                                                                                                            |
| ``HXL_REQUIRED_PROPERTY_NOT_FOUND`` | ``900``    | A property required in the schema is not found on the node.                                                                                                       |
| ``HXL_UNKNOWN_PROPERTY``            | ``910``    | A property not declared in the schema is found on the node.                                                                                                       |
| ``HXL_CANNOT_DESERIALIZE_NODE``     | ``1000``   | (Optional) If a deserializer is missing instructions for a specific node.                                                                                         |
