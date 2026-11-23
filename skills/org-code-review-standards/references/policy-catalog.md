# Organization Review Policy Catalog

Check added and modified code in the diff against the policies below. When a policy is violated, prefix the finding body with the exact bracketed Policy ID.

## Policies

### `[ORG-REV-001]` Outbound HTTP calls must set an explicit timeout

- **Applies to:** Go (`http.Get`, `http.Post`, `&http.Client{}` without `Timeout`), Python (`requests.get`/`post` or `httpx.get`/`post` without `timeout=`), JavaScript/TypeScript (`fetch(...)` without an `AbortSignal.timeout(...)` or `AbortController` signal).
- **Why:** An outbound call without a timeout can hang worker goroutines or event loops indefinitely when an upstream stalls.
- **Severity:** `warning` (or `critical` on a synchronous request-handling path).
- **Finding prefix:** `[ORG-REV-001]`

### `[ORG-REV-002]` Never log credentials, tokens, or authorization headers

- **Applies to:** Any logging call (`slog`, `log`, `logger`, `console.log`, `print`) that interpolates or passes variables named `token`, `secret`, `password`, `api_key`, `apiKey`, or `authorization` / `Authorization` header values directly.
- **Why:** Raw secrets in application logs leak into log aggregators and break credential containment.
- **Severity:** `critical`.
- **Finding prefix:** `[ORG-REV-002]`

### `[ORG-REV-003]` New `TODO` / `FIXME` comments must reference a tracking issue

- **Applies to:** Newly added `TODO` or `FIXME` comments that do not include an issue key matching `[A-Z]{2,10}-[0-9]+` (for example, `TODO(ENG-123): ...`) or a GitHub/GitLab issue URL.
- **Why:** Untracked `TODO`/`FIXME` comments accumulate as unowned technical debt.
- **Severity:** `minor`.
- **Finding prefix:** `[ORG-REV-003]`

### `[ORG-REV-004]` Do not leave raw debug print statements in production source files

- **Applies to:** Newly added `console.log(...)`, `fmt.Println(...)`, or bare `print(...)` calls in non-test, non-CLI source files where a structured logger is expected.
- **Why:** Unstructured stdout writes bypass log levels, correlation IDs, and log filtering in production.
- **Severity:** `minor`.
- **Finding prefix:** `[ORG-REV-004]`
