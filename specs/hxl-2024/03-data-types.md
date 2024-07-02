# Data types

## String

String values must be defined between quotation marks.

````text
<Player> MainCharacter
    name: "John Doe"
````

Please reference the **Rules** section for behavior-specific definitions.

## Integer

````text
<Player> MainCharacter
    int: 45
````

Values can be negative or positive.

Scientific notation, hexadecimal and similar forms are NOT supported.

## Floating-point value

````text
<Player> MainCharacter
    float: -10.5
````

Values can be negative or positive.

Scientific notation, hexadecimal and similar forms are NOT supported.

## Reference

A property can be a reference to another node.

To indicate a reference, the property name must be suffixed by ``&``.

The value of the property is the exact name of the node.

````text
<Player> MainCharacter
    age: 45
    
<Enemy> Monster
    target&: MainCharacter
````

## Array

Arrays are denoted by suffixing the property name with ``[]``.

Values are declared between ``{`` and ``}``, and are separated by comma (``,``).

````text
<Cube3D> CubeA
    position[] = { 2, 4, -2 }
````

As of 2024, only strings, integer and floating-point values are allowed as array values.
