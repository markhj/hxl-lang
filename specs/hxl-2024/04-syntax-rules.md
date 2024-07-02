# Syntax rules

Each rule has their own section below.
Most rules must return an error code, when the criteria aren't met,
and that will be listed as "Error code: ``HXL_ERROR_CODE``".

> In implementations, it's recommended to reference the rule name (e.g. ``GEN.001``)
> at the point where it is implemented, and as well in unit tests.

Almost all the rules mentioned below should fit within the tokenization
or grammar validation layer, of an implementation.

## GEN.001: No content

There MUST be content in an HXL source. AT LEAST one (valid) node must be declared.

Error code: ``HXL_EMPTY``

## GEN.002: End of file must be empty line

An HXL source MUST end with an empty line.

Error code: ``HXL_INVALID_EOF``

Valid:

````text
<NodeType> NodeName
    key: "value"
    
````

Invalid:

````text
<NodeType> NodeName
    key: "value"
````

## GEN.003: Ignoring ``\r``

The presence of ``\r`` is allowed in HXL sources, but MUST be completely ignored in
interpretation. Whenever we speak about "new line", it is ALWAYS and ONLY denoted
by ``\n``.

## NODE.001: Node type and name

A node declaration requires a node type and a name, provided on the form: ``<Type> Name``

Error code: ``HXL_INVALID_NODE_FORM``

Valid example:

````text
<Player> MainCharacter
````

## NODE.002:

There MUST be a single whitespace (``\s``) between the node type and its name.

Error code: ``HXL_ILLEGAL_WHITESPACE``

Valid:

````text
<Player> MainCharacter
````

Invalid:

````text
<Player>MainCharacter
````

## NODE.003: Tab before property

Every node property must be preceded by EXACTLY one tab (``\t``)

Error code: ``HXL_ILLEGAL_WHITESPACE``

Valid:

````text
<NodeType> NodeName
    key: 10
````

Invalid:

````text
<NodeType> NodeName
key: 10
````

## NODE.004: First ``:`` separates key and value

The first colon (``:``) marks where the key-value pair splits.
The first part is the key, the second is the value.

Error code: ``HXL_INVALID_PROPERTY_FORM``

Valid:

````text
<NodeType> NodeName
    key: "value"
````

Invalid:

````text
<NodeType> NodeName:
    key = "value"
````

## NODE.005: Whitespace between key and ``:``

Any type of whitespace between property name and ``:`` is NOT allowed.

Error code: ``HXL_ILLEGAL_WHITESPACE``

Valid:

````text
<NodeType> NodeName
    key: "value"
````

Invalid:

````text
<NodeType> NodeName
    key : "value"
````

## NODE.006: Whitespace after ``:``

A single whitespace ``\s`` MUST succeed ``:`` and precede the value.

Error code: ``HXL_ILLEGAL_WHITESPACE``

Valid:

````text
<NodeType> NodeName
    key: "value"
````

Invalid:

````text
<NodeType> NodeName
    key:"value"
````

## NODE.007: Value part non-string literals

Everything after the ``:`` (except for the initial, required whitespace) MUST
be considered part of the value, unless a ``#`` outside a string literal is encountered.

That means that reading a value ends at either ``\n`` (new line) or ``#`` outside a
string literal (comment)

## NODE.008: Value part for string literals

In the case of string literals, the value is everything scanned between
two non-escaped quotation marks.

A comment is allowed after the value. But if there is no comment,
an ``HXL_ILLEGAL_WHITESPACE`` error must be raised.

## NODE.009: Whitespace between end-of-string and comment

EXACTLY one whitespace (``\s``) must be placed between the end-of-string quotation mark
and the beginning of a comment.

Valid:

````text
  key: "value" # Comment
````

Invalid:

````text
  key: "value"# Comment
````

## NODE.010: Node type format

Node type format must satisfy: ``([A-Z][a-z0-9]+){1,}``

Error code: ``HXL_INVALID_NODE_TYPE``

Valid:

````text
<Cube3D> Box
````

Invalid:

````text
<Node:Type> NodeName
````

## NODE.011: Node name format

A node name must satisfy: ``([A-Z][a-zA-Z0-9]+)``

Error code: ``HXL_INVALID_NODE_NAME``

Valid:

````test
<NodeType> Node1
````

Invalid:

````test
<NodeType> Node_1
````

## NODE.012: Property name format

Property keys must satisfy: ``[a-z][a-z_]+``

Error code: ``HXL_INVALID_PROPERTY_KEY``

Valid:

````text
<Player> MainCharacter
    first_name: "John"
    last_name: "Doe"
    age: 45    
````

## NODE.013: Case-sensitivity

Names of nodes and properties are to be treated as case-sensitive, at all times.

## STR.001: Escaping characters

Character escapes are preceded by backslash, for example ``\"``.
Any character can be escaped.

## STR.002: ``#`` within string literal

Within a string literal ``#`` MUST NOT be interpreted as the beginning of a comment.

This means that:

````text
<NodeType> NodeName
    key: "Hello # World" # Comment here
````

In this snippet, the value of ``key`` is ``Hello # World``

## STR.003: ``:`` within string literal

Within a string literal ``:`` MUST NOT cause tokenization error, and must be considered part of the string.

Error code: ``HXL_UNEXPECTED_TOKEN``

That means in the following:

````text
<NodeType> NodeName
    value: "Hello : World"
````

That the value is ``Hello : World``.

## STR.004: ``\n`` within string literal

New-line ``\n`` is NOT allowed within a string literal.

Error code: ``HXL_ILLEGAL_STRING``

Invalid:

````text
  key: "hello
  world"
````

````text
  key: "Hello\nworld"
````

## INT.001: Integer notation

Integer values MUST be given on the form ``x`` or ``-x``.

As of _HXL.2024_, scientific, hexadecimal and other numeric notations are NOT supported.

Valid:

````text
  value: -5
````

## FLOAT.001: Floating-point value notation

Float values are ONLY valid when a decimal point and at least one decimal are present.
(example: ``-5.05``)

Error code: ``HXL_ILLEGAL_FLOAT``

As of 2024, scientific, hexadecimal and other numeric notations are NOT supported.

## FLOAT.002: Casting from integer notation not allowed

Values on the form ``-5`` or ``5`` MUST NOT be automatically cast to floating-point value.

Error code: ``HXL_ILLEGAL_FLOAT``

> NOTE: This validation can usually not be done until validating against a schema.

## ARR.001: Whitespace around array brackets

Array values MUST be encapsulated between ``{`` and ``}`` with EXACTLY
one whitespace character (``\s``) separating the braces from the values.

Error code: ``HXL_ILLEGAL_WHITESPACE``

Valid:

````text
  arr[]: { 1, 2, 3 }
````

Invalid:

````text
  arr[]: {1, 2, 3}
````

## ARR.002: Whitespace between array values

Array values MUST have EXACTLY one whitespace character (``\s``) preceding them,
and be separated by a comma (``,``).

Error code: ``HXL_ILLEGAL_WHITESPACE``

Valid:

````text
  arr[] = { 1, 2, 3 }
````

Invalid:

````text
  arr[] = { 1,2,3 }
````

## ARR.003: Supported types in array value

Array values MUST be either string, integer or floating-point values.

Nested arrays and references are NOT supported.

Error code: ``HXL_ARRAY_UNKNOWN_TYPE``

## REF.001: Treat as reference

When a property key is suffixed with ``&``, it must be treated as a
reference, and semantic, as well as schema validation, must apply.

## REF.002: Whitespace around ``&``

There MUST NOT be any whitespace surrounding the ``&`` delimiter.

Error code: ``HXL_ILLEGAL_WHITESPACE``

Valid:

````text
  ref&: NodeName
````

Invalid:

````text
  ref &: NodeName
````

## CMT.001: Scanning comments

Any text following ``#`` MUST be ignored, EXCEPT when the character is found inside a string literal.

Example:

````text
    key: "Hello # World" # Comment
````

In the above, ``key``'s value is ``Hello # World``.

`` # Comment`` SHOULD be ignored.

## CMT.002: Whitespace around ``#``

There MUST be exactly one whitespace ``\s`` on each side of ``#``.

Error code: ``HXL_ILLEGAL_WHITESPACE``

Valid:

````text
  key: "value" # Comment
````

Invalid:

````text
  key: "value" #Comment
````

````text
  key: "value"# Comment
````

## CMT.003: Empty comments

Comments must not be empty, this is to be considered a parsing error.

Error: ``HXL_ILLEGAL_COMMENT``

Invalid:

````text
  key: "value" #
````

## INHR.001: Whitespace around ``<=``

There MUST be exactly ONE whitespace character (``\s``) on each side of the ``<=`` token.

Error: ``HXL_ILLEGAL_WHITESPACE``

Valid:

````text
<NodeType> A <= B
````

Invalid:

````text
<NodeType> A<=B
````
