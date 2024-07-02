# Semantic rules

Semantic rules are allocated with numbers between ``200`` and ``399``.

## REF.200: Referenced nodes must exist

Referenced nodes MUST exist.

Error code: ``HXL_NODE_REFERENCE_NOT_FOUND``

Valid:

````text
<NodeType> A
    key: "value"
    
<NodeType> B
    ref&: A
````

Invalid:

````text
<NodeType> A
    key: "value"
    
<NodeType> B
    ref&: NonExistingNode
````

## REF.201: Nodes must be declared at time of being referenced

Referenced nodes MUST be declared in the HXL source, at the time they
are being referenced.

Error code: ``HXL_NODE_REFERENCE_NOT_FOUND``

Valid:

````text
<NodeType> A
    key: "value"
    
<NodeType> B
    ref&: A
````

Invalid:

````text
<NodeType> B
    ref&: A
    
<NodeType> A
    key: "value"
````

Notice here how ``A`` is declared later, but already referenced in ```B``.
This is NOT allowed, even though ``A`` exists within the source.

## REF.202: Circular references

Circular references MUST NOT occur.

Error code: ``HXL_CIRCULAR_NODE_REFERENCE``

Invalid:

````text
<NodeType> A
    ref&: B

<NodeType> B
    ref&: A
````

> Note: If you have properly implemented ``REF.201`` it will catch
> this issue. However, some may choose to run validation ``REF.202``
> first, to provide more clarity to the user about what they are doing.

## REF.203: Self-referencing

A node CANNOT reference itself.

Error code: ``HXL_ILLEGAL_REFERENCE``

Invalid:

````text
<NodeType> A
    ref&: A
````

## INHR.200: Node types must match 

An inherited node MUST have the EXACT same node type.

Error code: ``HXL_INHERIT_DIFF_TYPES``

Valid:

````text
<Enemy> Monster1
    health: 100
    
<Enemy> Monster2 <= Monster1
````

Invalid:

````text
<Player> MainCharacter
    health: 100
    
<Enemy> Monster2 <= MainCharacter
````

## INHR.201: Inherited nodes must exist

An inherited node MUST exist.

Error code: ``HXL_NODE_REFERENCE_NOT_FOUND``

Invalid:

````text
<Player> MainCharacter
    health: 100
    
<Enemy> Monster2 <= NonExistingNode
````

## INHR.202: Inherited nodes must already be declared

An inherited node MUST be declared earlier in the file.

Error code: ``HXL_NODE_REFERENCE_NOT_FOUND``

Valid:

````text
<Enemy> Monster1
    health: 100
    
<Enemy> Monster2 <= Monster1
````

Invalid:

````text
<Enemy> Monster1 <= Monster2
    
<Enemy> Monster2
    health: 80
````

## INHR.203: Self-inheritance

A node CANNOT inherit itself.

Error code: ``HXL_ILLEGAL_INHERITANCE``

Invalid:

````text
<Enemy> Monster1 <= Monster1
    health: 100
````

## NODE.200: Node name uniqueness

Node names MUST be unique.

Error code: ``HXL_NON_UNIQUE_NODE``

Valid:

````text
<NodeType> A

<NodeType> B
````

Invalid:

````text
<NodeType> A

<NodeType> A
````

## NODE.201: Node property uniqueness

A node's properties MUST be unique.

Error code: ``HXL_NON_UNIQUE_PROPERTY``

Valid:

````text
<Person> SomePerson
    age: 100
    bank_balance: -600.0
````

Invalid:

````text
<Person> SomePerson
    age: 100
    age: 130
    bank_balance: -600.0
````

> Note: We encourage strict enforcement, therefore it's not sufficient to
> make a design decision whether first or last declaration would take effect.
> Having duplicate node property names MUST be considered illegal.

## ARR.200: Mixed types in arrays

Arrays values MUST be of identical type.

Error code: ``HXL_ARRAY_MIXED_TYPES``

Valid:

````text
  arr[] = { 1, 2, 3 }
````

````text
  arr[] = { -0.5, 1.3, 2.0 }
````

````text
  arr[] = { "a", "b", "c" }
````

Invalid:

````text
  arr[] = { 1, 2.0, 3 }
````

````text
  arr[] = { "Hello", 2, 3 }
````