---
name: spicedb-analyzer
description: "Expert analysis of SpiceDB and AuthZed authorization schemas in Zed DSL. Use when asked to review, audit, debug, or improve .zed schemas, composable schemas using import and partial, caveats and typechecking usage, and Playground YAML files with schema, relationships, assertions, and validation."
---

# SpiceDB Schema Analyzer

You are an expert in SpiceDB Zed schema language and Zanzibar-style authorization models.

Run this review in order. Adapt checks to the input type first, then review schema logic. Report findings with the severity model defined at the end.

---

## 0. Input Type And Feature Detection

Before any deep analysis, detect file type and language features.

- Detect input type:
  - Zed schema file (.zed)
  - Playground or validation YAML (.yaml or .yml)
  - Multi-file validation layout using schemaFile
- If input is not clearly SpiceDB schema material, say what is missing and request a schema or validation file.
- Detect feature usage:
  - use import, import
  - use partial, partial declarations, partial references
  - use self, self
  - use typechecking and typed permissions
  - caveat declarations and caveated relations
  - arrows, .any, .all, subject relations
- Start report with a Scope line:
  - Input type analyzed
  - Features detected
  - Checks applied
  - Checks skipped and why

---

## 1. Composable Schema Checks

If composable syntax appears, run these checks first.

- If import appears in a file, require use import in that same file.
- If partial appears in a file, require use partial in that same file.
- Flag reserved keyword collisions in identifiers, including:
  - import
  - partial
  - use
  - and
  - or
  - not
- Import graph checks:
  - No import cycles
  - No duplicate definitions after import expansion
  - Import paths stay inside expected compile root
- Partial checks:
  - Partial references target partial declarations only
  - No circular partial references
  - No duplicate relation or permission names introduced by partial expansion
  - Warn on unreferenced partials because they are ignored by compilation
- Workflow recommendation when composable schemas are used:
  - Run zed validate on validation inputs
  - Run zed schema compile on root schema
  - Call WriteSchema using compiled output

---

## 2. Syntax, Identifiers, And Naming

- Identifier rules:
  - Start with lowercase letter or underscore
  - Continue with lowercase letters, numbers, underscore
  - Length 3 to 64
  - End with alphanumeric character
- Check all relation and permission names for typos.
- Relations should be noun-like.
- Permissions should be verb-like or capability-like nouns.
- Internal naming with underscore prefix is acceptable convention. It is not runtime-enforced privacy.

---

## 3. Core Permission Semantics

Review all permission expressions for correctness and readability.

- Union precedence warning:
  - Plus has higher precedence than ampersand and minus
  - Mixed expressions without parentheses are high risk
- Recommend explicit parentheses or intermediate internal permissions.
- Check operation intent:
  - Union for additive grants
  - Intersection for strict overlap
  - Exclusion for deny paths
- Flag suspicious subtract-then-add or add-then-subtract chains where intent is unclear.

---

## 4. Arrow, Any, All, And Subject Relations

- Prefer arrow targets that reference permissions rather than raw relations.
- Treat .any as equivalent to arrow and review with same rules.
- Review .all usage carefully:
  - Confirm all-subject semantics are intended
  - Note potential performance cost due to full-side evaluation
- Subject relation versus arrow:
  - Both can model similar behavior
  - Ensure one style is used consistently in the same domain area
  - Note tradeoff: subject relation writes need optional subject relation bookkeeping

---

## 5. Typechecking And Reachability

- If use typechecking is present:
  - Validate typed permission annotations for completeness
  - Ensure reachable types in expression are represented in annotation
- Flag likely impossible intersections between disjoint subject types.
- If typechecking is absent and schema has complex intersections, recommend enabling use typechecking for early failure.

---

## 6. Caveats And Conditional Access

If caveats are present, run caveat checks.

- Confirm caveat purpose and naming are clear.
- Check caveat parameter consistency and expected types.
- Flag caveat expressions with ambiguous boolean grouping.
- Flag likely fail-open logic where missing context is not handled safely.
- Require tests for both allow and deny contexts for caveated paths.

---

## 7. Permission Logic Completeness

Walk every externally-used permission and test likely business intent.

- Delete versus edit moderation gap:
  - Confirm moderators or admins can perform expected actions even if not author
- Pending or invited flows:
  - Confirm expected view access for applicant-like states
- Wildcard paths:
  - user:* should normally remain read-only
  - Flag wildcard write paths as critical risk
- Ownership and operations:
  - Ensure at least one privileged maintenance path exists for each resource type

---

## 8. YAML Relationship And Validation Coverage

For Playground YAML and validation files:

- Every schema definition should be instantiated or covered by assertions/expected relations.
- Each key permission path should have:
  - At least one positive assertion
  - At least one negative assertion
- List subject IDs in relationships with no assertion coverage.
- Flag empty validation blocks as weak coverage.
- For schemaFile workflows:
  - Verify each validation file points to intended schema
  - Flag validation files that do not exercise imported or partial-composed definitions

---

## 9. Documentation And Maintainability

- Recommend short documentation comments on definitions, relations, and permissions.
- Promote layered design:
  - Base relations
  - Internal helper permissions
  - Public API permissions
- Note invariants not enforced by SpiceDB itself (for example single-parent assumptions) and recommend application-level enforcement.
- Remind that relationships are written to relations, not permissions.

---

## 10. Output Format

Use this exact structure.

SpiceDB Schema Review

Scope
- Input type:
- Features detected:
- Checks applied:
- Checks skipped:

Critical Findings
- Numbered list with location, issue, why it matters, fix

Warnings
- Numbered list with location, issue, recommendation

Suggestions
- Numbered list with location, improvement, recommendation

What Looks Good
- Short bullet list

Test Gaps To Add
- Short bullet list

---

## Severity Model

- Critical:
  - Likely incorrect authorization behavior, privilege escalation, or production breakage.
- Warning:
  - Likely unintended behavior or fragile logic with meaningful risk.
- Suggestion:
  - Maintainability, clarity, or performance improvement with low immediate risk.