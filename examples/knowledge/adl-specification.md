# ADL - Assistant Description Language

## Overview

ADL (Assistant Description Language) is a YAML-based specification language for defining conversational AI assistants. It provides structured, validatable definitions that separate concerns and enable reusable configurations.

## Core Principles

1. **Declarative over Imperative**: Behaviors are declared, not programmed
2. **Human-Readable**: YAML format accessible to non-programmers
3. **Structured**: Formal schemas enable validation
4. **Modular**: Concerns separated into distinct sections

## Main Sections

### Identity
Defines personality, communication style, and distinctive traits.

### Role
Specifies function, expertise areas, and action boundaries.

### Behavior
Event-triggered responses and interaction protocols.

### Tools
Commands, options, and decorators for extended functionality.

### Knowledge
External documents injected into the assistant's context.

## Educational Applications

ADL is particularly suited for creating educational tutors because:

1. **Personalization**: Identity and behavior sections allow tailoring to learner needs
2. **Expertise Modeling**: Role section defines subject matter knowledge
3. **Scaffolding**: Protocols can implement pedagogical strategies
4. **Boundaries**: Clear limits on what the tutor can/cannot do

## Example: Math Tutor

```yaml
identity:
  display_name: "Professor Euler"
  personality: |
    Patient and encouraging mathematics tutor who believes
    every student can understand math with the right approach.
  communication_style:
    tone: friendly
    verbosity: balanced

role:
  type: tutor
  expertise:
    - domain: "Algebra"
      level: expert
    - domain: "Calculus"
      level: advanced
  responsibilities:
    - "Explain mathematical concepts"
    - "Guide problem-solving step by step"
    - "Provide practice problems"
```

## Key Publications

- Pernías Peco, P.A. & Escobar Esteban, M.P. (2025). "ADL: A Description Language for Conversational AI Assistants." ICERI 2025.

## Resources

- Repository: https://github.com/ppernias/adl
- License: CC-BY-4.0
