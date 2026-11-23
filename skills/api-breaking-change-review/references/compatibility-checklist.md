# API Compatibility Checklist

Check modified API routes, schemas, and serialized payloads for the breaking changes below.

### `[ORG-API-001]` Removed or renamed response fields without versioning

- **Trigger:** An existing JSON/GraphQL response field is removed, renamed, or changes type (for example, `string` to `number`, or non-null to nullable) on an existing versioned or unversioned endpoint.
- **Severity:** `critical`.
- **Finding prefix:** `[ORG-API-001]`

### `[ORG-API-002]` New required request fields on existing endpoints

- **Trigger:** A newly added request body field, query parameter, or header is marked required without a backward-compatible default for existing callers.
- **Severity:** `critical`.
- **Finding prefix:** `[ORG-API-002]`

### `[ORG-API-003]` Narrowed enum values on request or response schemas

- **Trigger:** An existing allowed enum value is removed from a request schema, or a new undocumented enum value is emitted where strict clients switch exhaustively without a fallback.
- **Severity:** `warning`.
- **Finding prefix:** `[ORG-API-003]`
