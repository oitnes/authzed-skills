# authzed-skills

A collection of skills for working with [SpiceDB](https://authzed.com/docs/) and AuthZed permission schemas

## Available skills

| Skill | Description |
|---|---|
| `spicedb-analyzer` | Expert analysis of SpiceDB .zed schemas, composable schemas, Playground YAML files, and validation setups. Detects bugs, logic gaps, operator-precedence issues, composable schema risks, typechecking gaps, caveat logic errors, and test-coverage holes. |

## Installation

### Via plugin marketplace (recommended)

```shell
# 1. Register the marketplace (once)
/plugin marketplace add https://github.com/oitnes/authzed-skills

# 2. Install the skill
/plugin install spicedb-analyzer@authzed-skills
```

## Usage

Once installed, invoke the skill explicitly:

```
Use the /spicedb-analyzer skill to review @path/to/schema.zed
```

Or just describe what you want — Agent will activate it automatically when the task matches:

```
Review my SpiceDB schema for bugs and logic gaps
Audit my composable schema setup
Check if my typechecking is complete
```

## What the skill checks

### Core Schema Checks
- Syntax and identifier validity
- Relation and permission naming conventions
- Operator precedence in expressions (union, intersection, exclusion)
- Arrow operator usage and performance implications
- .any and .all operators and their semantics

### Permission Logic
- Delete versus edit moderation gaps
- View access for pending or invited states
- Wildcard path restrictions and write risks
- Unreachable intersections and type mismatches
- Ownership maintenance operations

### Advanced Features
- Composable schema syntax (import, partial, partial references)
- Reserved keyword collisions in identifiers
- Import cycles and duplicate definitions
- Typechecking completeness and annotation safety
- Caveat definitions and caveated relation testing
- Subject relations versus arrow consistency

### Relationship and Validation Coverage
- YAML Playground relationship instantiation
- Assertion coverage for each permission path
- Untested definitions and actors
- Multi-file validation workflows using schemaFile
- Empty validation blocks

### Design and Maintainability
- Documentation comments on definitions, relations, permissions
- Intermediate helper permission patterns
- Single-parent invariants
- Layered schema design (base relations, internal permissions, public API)
- Relationship target clarity (relations only, not permissions)

## Supported Inputs

- Single .zed schema files
- Playground YAML files with schema, relationships, assertions, validation
- Multi-file composable schemas using import and partial
- Multi-file validation setups using schemaFile
- Mixed-feature schemas with self, typechecking, caveats

## Output

All findings are reported in plain ASCII text suitable for any terminal. The skill categorizes issues as:

- Critical: Authorization behavior, privilege escalation, or production breakage
- Warning: Likely unintended behavior or fragile logic
- Suggestion: Maintainability, clarity, or performance improvements

## License

MIT
