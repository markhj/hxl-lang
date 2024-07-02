# Schema validation

You can read more about the schema under **Implementation nodes**.

Schema validation errors are allocated betwen ``500`` and ``599``.

But the gist of it is that every implementation must provide a schema,
which is either natively defined, or can be defined by an end-user.

## SCHEMA.500: Invalid node type

When a node type isn't explicitly defined in the schema.

Error code: ``HXL_UNKNOWN_NODE_TYPE``

Valid:

````text
<DeclaredType> NodeName
````

Invalid:

````text
<UndeclaredType> NodeName
````

## SCHEMA.510: Wrong data type

When a property value is given in a valid form, but doesn't match
what was expected by the schema.

Error code: ``HXL_ILLEGAL_DATA_TYPE``


## SCHEMA.511: Multiple data types not supported

A schema MUST NOT allow multiple data types on a single property.

## SCHEMA.520: Required property not found

Required node property not found.

Error code: ``HXL_REQUIRED_PROPERTY_NOT_FOUND``

Valid:

````text
<NodeType> NodeName
    required_property: 10
````

Invalid:

````text
<NodeType> NodeName
    # required_property not declared
````

## SCHEMA.522: Unrecognized property

A property NOT found in the schema provided as node property.

Error code: ``HXL_UNKNOWN_PROPERTY``

Invalid:

````text
<NodeType> NodeName
    undeclared_property: 10
````
