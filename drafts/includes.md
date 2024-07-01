# Draft: Include a file

> **Important**:
> This is a draft of an HXL feature which is not yet part of the official specification

## Including a HXL file

Must be declared in the beginning of the file to enforce some structure and
overview.

````text
include filename.hxl
include another.hxl

<NodeType> A
    key: value
    # And so on...
````
## Including a stream

````text
<Shader> MyShader
    vertex<<: vertex-shader.glsl
    fragment<<: fragment-shader.glsl
````
