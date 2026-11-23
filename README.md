# Sample Organization Skills Repository

A minimal, ready-to-connect organization skills repository for testing skill discovery, on-demand loading (`load_capability`), and bundled reference reads (`read_skill_file`) in a code review workflow.

## Structure

Skills live one level down under `skills/<name>/SKILL.md` on the default branch (`main`):

```text
skills/
├── org-code-review-standards/
│   ├── SKILL.md
│   └── references/
│       └── policy-catalog.md
└── api-breaking-change-review/
    ├── SKILL.md
    └── references/
        └── compatibility-checklist.md
```

- **`org-code-review-standards`**: Broadly scoped to every PR/MR review so the review agent reliably loads it and reads `references/policy-catalog.md`. Findings triggered by it are prefixed with `[ORG-REV-001]`–`[ORG-REV-004]`, making pickup immediately recognizable in the posted review.
- **`api-breaking-change-review`**: Narrowly scoped to API handler/schema changes so you can also verify selective, on-demand loading. Findings triggered by it are prefixed with `[ORG-API-001]`–`[ORG-API-003]`.

## Setup

1. Ensure the `agent.skills` feature flag is enabled for your organization.
2. Grant your organization's GitHub App installation read access to this repository (if the installation is scoped to selected repositories).
3. In your organization's **Settings → Agent instructions**, select this repository as the **Skills repository** and save.

## How to test in a code review run

Open a test PR in any other connected repository of the same organization with a small change that deliberately violates one or more catalog rules. For example:

### Python example

```python
import logging
import requests

logger = logging.getLogger(__name__)


def fetch_profile(user_id: str, api_key: str) -> dict:
    # TODO: cache this response later
    logger.info("Fetching profile with api_key=%s", api_key)
    resp = requests.get(f"https://example.com/users/{user_id}")
    return resp.json()
```

### Go example

```go
package sample

import (
	"fmt"
	"net/http"
)

// TODO: handle pagination
func FetchProfile(userID, apiKey string) (*http.Response, error) {
	fmt.Println("debug fetch with apiKey:", apiKey)
	return http.Get("https://example.com/users/" + userID)
}
```

### TypeScript example

```ts
export async function fetchProfile(userId: string, apiKey: string) {
  // TODO: add retry logic
  console.log("fetching profile with apiKey:", apiKey);
  const res = await fetch(`https://example.com/users/${userId}`);
  return res.json();
}
```

## What to verify

1. **Run logs / activity feed**:
   - The review step logs the fetched skills repository commit SHA and the loaded skill count (2 skills, 0 unparseable).
   - The agent calls `load_capability` with `id: "org-code-review-standards"`.
   - The agent calls `read_skill_file` with `skill: "org-code-review-standards"` and `path: "references/policy-catalog.md"`.
2. **Posted review**:
   - Findings include the unmistakable policy prefixes from `references/policy-catalog.md`:
     - `[ORG-REV-001]` (missing HTTP timeout)
     - `[ORG-REV-002]` (logging `api_key` / `apiKey`)
     - `[ORG-REV-003]` (untracked `TODO` comment)
     - `[ORG-REV-004]` (raw `console.log` / `fmt.Println`)
