---
name: api-breaking-change-review
description: Backward-compatibility review checklist for changes touching HTTP API handlers, OpenAPI/GraphQL schemas, or public request/response models.
---

# API Breaking Change Review

Load this skill whenever a pull request or merge request modifies HTTP route handlers, OpenAPI/Swagger specifications, GraphQL schemas, or serialized request/response structs and types.

## Steps

1. Read `references/compatibility-checklist.md` using `read_skill_file`.
2. Verify every modified endpoint, schema field, status code, and error envelope against the checklist.
3. For each breaking change found, prefix the finding body with the exact bracketed Policy ID from the checklist (for example, `[ORG-API-001] ...`).
