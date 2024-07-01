# Draft: Abstract nodes

> **Important**:
> This is a draft of an HXL feature which is not yet part of the official specification

Similar to abstract classes. Define a node which isn't given in the final
result, but from which child nodes can inherit properties.

````text
<Abstract:Enemy> EnemyTypeA
    health: 100
    
<Enemy> MonsterOne <= EnemyTypeA
    # health is implicitly inherited
    position[]: { 8, 0 , 8 }
````

## Behavior

- ``EnemyTypeA`` will not be provided in the final output

## Rules

- There can be only ONE abstract per node type
