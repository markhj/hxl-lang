# HXL.2024

**HXL** is a data interchange format similar to JSON or YAML, but sporting
useful features such as inheritance and references.

It was originally designed as a data storage for a game engine,
containing information about objects, materials and lights.

## Design goal

The design goal is to make HXL:

- Useful
- Streamlined
- Easy to parse

## How it works

Interpretations of HXL are separated into these stages:

- Tokenization
- Verification of grammar
- Validation of semantics
- Validation against a schema
- Deserialization, providing the parsed data in format
  suitable to the implementation language.

Please refer the repository frontpage to see current implementations.
