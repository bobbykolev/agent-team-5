<!--
SYNC IMPACT REPORT
==================
Version change: 0.0.0 → 1.0.0 (initial ratification)
Modified principles: N/A (initial creation)
Added sections:
  - Core Principles (4 principles)
  - Development Workflow
  - Quality Standards
  - Governance
Removed sections: None
Templates requiring updates:
  - .specify/templates/plan-template.md: ✅ compatible (Constitution Check section exists)
  - .specify/templates/spec-template.md: ✅ compatible (requirements align)
  - .specify/templates/tasks-template.md: ✅ compatible (phase structure aligns)
  - .specify/templates/checklist-template.md: ✅ compatible
  - .specify/templates/agent-file-template.md: ✅ compatible
Follow-up TODOs: None
-->

# Agent Team 5 Constitution

## Core Principles

### I. Simplicity First

Every solution MUST start with the simplest viable approach. Complexity is a liability that
requires explicit justification.

**Rules**:
- YAGNI (You Aren't Gonna Need It): Do not implement features until they are needed
- No premature abstractions: Three similar lines of code are better than a premature helper
- No speculative architecture: Design for current requirements, not hypothetical futures
- Minimal dependencies: Each external dependency MUST justify its inclusion

**Rationale**: Simple code is easier to understand, test, modify, and debug. Complexity
compounds over time and increases maintenance burden exponentially.

### II. Incremental Delivery

Features MUST be delivered in small, independently testable increments. Each increment
MUST provide demonstrable value.

**Rules**:
- User stories MUST be independently implementable and testable
- Each story MUST deliver a working slice of functionality (vertical slice)
- Foundational work MUST be minimal and focused on unblocking story delivery
- MVP first: Complete the highest-priority story before expanding scope

**Rationale**: Incremental delivery reduces risk, enables early feedback, and ensures
progress is always measurable and demonstrable.

### III. Explicit Over Implicit

All design decisions, requirements, and constraints MUST be documented explicitly.
Assumptions MUST be surfaced and validated.

**Rules**:
- Unclear requirements MUST be marked with `[NEEDS CLARIFICATION: reason]`
- Technical decisions MUST include rationale in plan documentation
- Dependencies between tasks MUST be explicitly stated
- Configuration and environment requirements MUST be documented

**Rationale**: Explicit documentation reduces misunderstandings, enables async collaboration,
and creates a traceable decision history.

### IV. Language-Agnostic Quality

Quality standards apply uniformly regardless of programming language or technology stack.
The principles in this constitution are technology-independent.

**Rules**:
- Test coverage expectations apply to all languages
- Code review standards are consistent across the codebase
- Documentation requirements are uniform
- Error handling and logging patterns MUST be consistent in spirit, adapted to language idioms

**Rationale**: A multi-language codebase requires consistent quality standards to maintain
coherence and enable contributors to work across different parts of the system.

## Development Workflow

### Planning Phase

1. Feature requests MUST be captured as specifications (`/speckit.specify`)
2. Specifications MUST include prioritized user stories with acceptance criteria
3. Implementation plans MUST pass Constitution Check before proceeding
4. Tasks MUST be organized by user story to enable independent delivery

### Implementation Phase

1. Work in priority order (P1 before P2 before P3)
2. Each user story SHOULD be completable without depending on lower-priority stories
3. Commit after each logical unit of work
4. Validate at each checkpoint before proceeding

### Review Phase

1. All changes MUST be reviewed against this constitution
2. Complexity additions MUST include justification in the Complexity Tracking table
3. Constitution violations MUST be resolved or explicitly justified before merge

## Quality Standards

### Testing

- Tests SHOULD cover critical paths and edge cases
- Test-first approach is RECOMMENDED but not mandated
- Integration tests are REQUIRED for cross-component communication
- Contract tests are REQUIRED for external API boundaries

### Documentation

- Public interfaces MUST have documentation
- Complex logic SHOULD include inline comments explaining "why" not "what"
- README files MUST be kept current with setup and usage instructions

### Observability

- Errors MUST be logged with sufficient context for debugging
- Structured logging is RECOMMENDED for production systems
- Performance-critical paths SHOULD include instrumentation

## Governance

### Amendment Process

1. Propose amendment with rationale
2. Document impact on existing specifications and plans
3. Update affected templates if principle changes affect their structure
4. Increment version according to semantic versioning

### Versioning Policy

- **MAJOR**: Backward-incompatible principle changes or removals
- **MINOR**: New principles added or existing principles materially expanded
- **PATCH**: Clarifications, wording improvements, typo fixes

### Compliance

- All PRs MUST verify alignment with constitution principles
- Constitution Check in plan-template.md MUST be completed before implementation
- Violations discovered during review MUST be resolved before merge

**Version**: 1.0.0 | **Ratified**: 2026-02-04 | **Last Amended**: 2026-02-04
