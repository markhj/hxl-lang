# Draft: Multi-line strings

> **Important**:
> This is a draft of an HXL feature which is not yet part of the official specification

Make it possible to have strings on multiple lines.

````text
<Shader> CustomShader
    vertex:
        in vec3 color;
        main() {
            // Do something
        }
````

## Considerations

- A delimiter which instructs the interpreter to expect a multi-line string.
- It has the potential to make HXL files messy.

## Rules

- There MUST be two ``\t`` prefixed each line.
- No content allowed on the line declaring the property.
