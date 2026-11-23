---
name: org-code-review-standards
description: Mandatory organization-wide code review standards and policy IDs. Load on every pull request or merge request review before finalizing findings.
---

# Organization Code Review Standards

Apply these organization-wide checks to every pull request or merge request under review, in addition to your normal defect analysis.

## Steps

1. Read `references/policy-catalog.md` using `read_skill_file` to get the active policy IDs, trigger conditions, and required finding format.
2. Inspect every added or modified line in the diff against each policy in the catalog.
3. For any violation you verify in the diff, report a finding following the normal review rubric and **prefix the finding body with the exact policy ID in square brackets** (for example, `[ORG-REV-001] ...`) so authors and reviewers can trace it back to the organization policy.
4. Only report violations that are actually present in the changed code; do not flag unchanged lines or files outside the diff.
