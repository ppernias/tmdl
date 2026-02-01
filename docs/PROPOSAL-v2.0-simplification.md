# TMDL v2.0 Simplification Proposal

## Principio guía: "Menos es más"

El TeamMate debe definir **quién es** (identidad estática), no **en qué trabaja** (contexto dinámico).

---

## 1. Redundancias identificadas

| Campo A | Campo B | Problema |
|---------|---------|----------|
| `metadata.context` | `context.organizational_context.type` | Ambos definen el contexto de uso |
| `metadata.intended_audience` | Implícito en context | Raramente útil |
| `identity.background` | `role.expertise` | El background repite las áreas de expertise |
| `context.resources.documents` | `knowledge` | Ambos definen documentos externos |
| `context.resources.references` | `knowledge` | Podría ser parte de knowledge |
| `metadata.contributors` | `context.team_members` | Solapamiento conceptual |
| `metadata.related` | - | Demasiado específico, raramente usado |
| `metadata.history` | - | Útil pero opcional para v2 |

### Protocols: demasiado granular

Actualmente hay **15 protocolos** definidos:
- on_task_assignment, on_contribution, on_review_request, on_help_request
- on_blocked, on_milestone, on_deadline_approaching
- on_greeting, on_farewell, on_help, on_unknown_request
- on_off_topic, on_error, on_appreciation, on_feedback

**Propuesta**: Reducir a 5-6 esenciales o consolidar en grupos.

---

## 2. Campos dinámicos → knowledge/

Estos campos cambian con el proyecto y deberían ser documentos externos:

| Campo actual | Razón | Documento propuesto |
|--------------|-------|---------------------|
| `context.project_description` | Evoluciona con el proyecto | `knowledge/project.md` |
| `context.project_domain` | Parte del proyecto | `knowledge/project.md` |
| `context.team_members` | Personas entran/salen | `knowledge/team.md` |
| `context.team_description` | Relacionado con team | `knowledge/team.md` |
| `context.timeline` | Cambia constantemente | `knowledge/timeline.md` |
| `context.organizational_context` | Puede cambiar | `knowledge/organization.md` |
| `context.constraints` | Varían según fase | `knowledge/organization.md` |
| `context.resources` | Documentos cambian | Integrar en `knowledge` |

---

## 3. Estructura propuesta TMDL v2.0

### YAML del TeamMate (estático)

```yaml
tmdl_version: "2.0"

# ─────────────────────────────────────────────────────────────
# METADATA: Información administrativa mínima
# ─────────────────────────────────────────────────────────────
metadata:
  id: string              # required
  name: string            # required
  description: string     # optional, breve
  version: string         # required
  status: enum            # draft|published|deprecated
  author: string          # required
  created: date
  updated: date
  language: string
  tags: [array]
  license: string

# ─────────────────────────────────────────────────────────────
# IDENTITY: Quién es el TeamMate
# ─────────────────────────────────────────────────────────────
identity:
  display_name: string    # required
  avatar: string
  personality: string     # required, narrativa
  communication_style:
    verbosity: enum       # concise|balanced|detailed
    tone: string|array    # formal|professional|technical|friendly...
    emoji_use: boolean
  quirks: [array]         # rasgos distintivos

# ─────────────────────────────────────────────────────────────
# ROLE: Qué hace el TeamMate
# ─────────────────────────────────────────────────────────────
role:
  type: string|array      # required, predefined or custom
  cognitive_style: enum   # adaptor|innovator|balanced (Kirton)
  expertise: [array]      # domains with level
  responsibilities: [array]
  boundaries:
    can_do: [array]
    cannot_do: [array]
    defers_to: [array]

# ─────────────────────────────────────────────────────────────
# COLLABORATION: Cómo trabaja con el equipo
# ─────────────────────────────────────────────────────────────
collaboration:
  contribution_style: string|array  # Benne & Sheats
  protocols:              # SIMPLIFICADO a 6 esenciales
    on_start: string      # inicio de sesión/tarea
    on_contribute: string # al hacer aportaciones
    on_review: string     # al revisar trabajo
    on_blocked: string    # cuando no puede avanzar
    on_feedback: string   # al recibir feedback
    on_error: string      # al cometer errores
  guardrails: [array]

# ─────────────────────────────────────────────────────────────
# TOOLS: Comandos y opciones
# ─────────────────────────────────────────────────────────────
tools:
  commands: [array]       # /command
  options: [array]        # /option value
  decorators: [array]     # +++modifier

# ─────────────────────────────────────────────────────────────
# KNOWLEDGE: Referencias a documentos externos
# ─────────────────────────────────────────────────────────────
knowledge:
  - id: project-context
    source: knowledge/project.md
    type: context
    inject: always

  - id: team-info
    source: knowledge/team.md
    type: context
    inject: always

  - id: timeline
    source: knowledge/timeline.md
    type: context
    inject: on_demand
```

### Documentos en knowledge/ (dinámicos)

#### knowledge/project.md
```markdown
# Project Context

## Domain
Educational Technology / AI in Education

## Description
Writing an academic paper that explores how context engineering...

## Goals
1. Define context engineering in the LLM era
2. Present ADL as a context specification language
...

## Current Phase
Writing - Literature Review
```

#### knowledge/team.md
```markdown
# Team

## Description
A small research team of two experienced academics...

## Members

### Pedro Pernías Peco
- **Role**: Lead Author
- **Responsibilities**: Research direction, Technical aspects
- **Contact**: p.pernias@gmail.com

### M. Pilar Escobar Esteban
- **Role**: Co-Author
- **Responsibilities**: Educational perspective, Validation
```

#### knowledge/timeline.md
```markdown
# Project Timeline

## Milestones

| Milestone | Date | Status |
|-----------|------|--------|
| Literature review complete | 2025-02-15 | pending |
| First draft complete | 2025-03-01 | pending |
| Internal review | 2025-03-15 | pending |
| Final submission | 2025-04-01 | pending |

## Current Focus
Completing the literature review section.
```

#### knowledge/organization.md
```markdown
# Organizational Context

## Organization
Universidad de Alicante
Departamento de Lenguajes y Sistemas Informáticos

## Type
Academic

## Constraints
- Paper must be original
- Follow target venue formatting guidelines
- All claims must be supported by evidence
```

---

## 4. Campos eliminados

| Campo | Razón de eliminación |
|-------|---------------------|
| `metadata.context` | Redundante con organizational_context |
| `metadata.intended_audience` | Raramente útil, implícito |
| `metadata.contributors` | Redundante con team.md |
| `metadata.related` | Demasiado específico |
| `metadata.rights_description` | Redundante con license |
| `identity.background` | Fusionado con personality |
| `context.*` (toda la sección) | Movido a knowledge/ |
| 9 protocols redundantes | Consolidados en 6 |

---

## 5. Comparación de complejidad

| Métrica | v1.1 | v2.0 propuesta |
|---------|------|----------------|
| Secciones principales | 7 | 6 |
| Campos en metadata | 15 | 10 |
| Campos en identity | 6 | 5 |
| Campos en context | 12+ | 0 (movido) |
| Protocols | 15 | 6 |
| **Total campos aprox.** | ~80 | ~45 |

---

## 6. Beneficios

1. **Separación clara**: YAML = identidad, knowledge/ = contexto
2. **Reutilización**: El mismo TeamMate en distintos proyectos
3. **Mantenimiento**: Actualizar timeline no requiere tocar YAML
4. **Simplicidad**: ~45% menos campos
5. **Legibilidad**: Documentos markdown son más fáciles de editar
6. **Colaboración**: El equipo puede editar project.md sin tocar el schema

---

## 7. Migración

Para TeamMates existentes:
1. Extraer campos dinámicos a archivos .md
2. Añadir referencias en knowledge
3. Eliminar sección context del YAML
4. Actualizar tmdl_version a "2.0"

---

## Decisión pendiente

¿Proceder con esta simplificación para TMDL v2.0?
