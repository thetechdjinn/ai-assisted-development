# Code Review Instructions

This file contains structured instructions for AI-assisted code reviews. It is designed to be loaded into an LLM's context only during code review sessions, not during implementation. Each section targets a specific review role so that multiple LLMs can cover different dimensions of code quality without overlapping.

## How to Use This File

### Providing Code for Review

When submitting code for review, provide the LLM with the following:

1. **The relevant section of this file.** Only load the section that matches the role you are assigning to that LLM. Do not load the entire file unless you are using a single LLM for all review roles.
2. **The diff output.** Use `git diff main...HEAD` (or the equivalent for your branch) to generate the changes being reviewed. This gives the reviewer focused context on what changed rather than requiring it to read entire files.
3. **Full file contents when needed.** If the diff is difficult to understand without surrounding context, include the full files that were modified. This is especially useful when reviewing architectural changes or when the diff touches many parts of a single file.
4. **The phase document.** Include the phase document so the reviewer understands what the code is supposed to accomplish. This allows it to verify that the implementation matches the intent, not just that the code is syntactically correct.

### Review Prompt Template

Use the following prompt structure when initiating a code review session:

```
I need you to perform a code review. Your review role is [Correctness / Security / Maintainability].

Please review the following code changes against the phase document provided.

Follow the instructions in the review document below for your specific role.

Provide your findings as:
1. A summary checklist with pass/fail status for each review area
2. Detailed narrative feedback for any items that did not pass or that warrant discussion

[Paste the relevant role section from this file]
[Paste the phase document]
[Paste the diff or file contents]
```

### Interpreting Results

After each LLM completes its review, look for:

- **PASS items.** No action needed. These confirm the code meets the criteria for that review area.
- **FAIL items.** These need to be addressed. Feed the specific failure details back to the development LLM for correction.
- **WARN items.** These are not blocking issues but are worth considering. Use your judgment on whether to address them now or defer them.

After corrections are made, re-submit the updated code to the same reviewer to confirm the issues have been resolved.

---

## Role 1: Correctness and Logic

**Assigned to: LLM 1 (External reviewer)**

This reviewer's job is to verify that the code does what it is supposed to do. It is focused on functional correctness, not style or structure.

### Review Checklist

For each item, respond with PASS, FAIL, or WARN and provide details for any non-PASS result.

**Requirements alignment:**
- [ ] The implementation matches the behavior described in the phase document.
- [ ] All acceptance criteria from the phase document are addressed in the code.
- [ ] No features or behaviors were added that are not described in the phase document.

**Logic correctness:**
- [ ] Conditional logic (if/else, switch, ternary) handles all expected cases.
- [ ] Loop constructs have correct bounds and termination conditions. No off-by-one errors.
- [ ] Null, undefined, empty, and special numeric values (NaN, Infinity) are handled where data could be absent or invalid.
- [ ] Type conversions and comparisons are intentional and correct. Integer vs float, string vs number, and similar type boundaries are handled explicitly.
- [ ] Functions return consistent types across all code paths. No paths that return mixed types (e.g., returning an object in one branch and undefined in another) unless the contract explicitly allows it.

**Data handling:**
- [ ] Data flows through the system as expected, from input to processing to output.
- [ ] Database queries return the expected results for the described use cases.
- [ ] Data transformations (mapping, filtering, aggregation) produce correct output.
- [ ] Boundary values and edge cases are handled (empty lists, maximum values, special characters).
- [ ] All columns, fields, and properties defined in the data model are accounted for. No relevant data is missing from queries, responses, or UI displays.
- [ ] Multi-step operations that must succeed or fail together are wrapped in transactions. A failure partway through does not leave data in an inconsistent state.

**Error scenarios:**
- [ ] The code behaves correctly when expected errors occur (network failures, invalid input, missing data).
- [ ] Error states do not leave the application in an inconsistent or broken state.
- [ ] Retry logic, if present, has appropriate limits and backoff behavior.

**Integration points:**
- [ ] API calls use the correct endpoints, methods, and parameters.
- [ ] Request and response formats match the expected contracts.
- [ ] Third-party library usage follows documented API conventions.
- [ ] Async operations are handled consistently. No missing awaits, unhandled promises, or mixing of async patterns (callbacks vs promises vs async/await) within the same module.

**Documentation alignment:**
- [ ] The code behavior matches what is described in the official project documentation (not just the phase document). If the implementation deviates from the documented behavior, either the code or the documentation needs to be updated.

### Narrative Instructions

After completing the checklist, provide a written summary addressing:

1. Any logic that is technically correct but fragile, meaning it works now but would break easily if assumptions change.
2. Cases where the code appears to work for the described scenarios but may fail for reasonable inputs that were not explicitly mentioned in the phase document.
3. Any assumptions the code makes that are not documented or validated.
4. Any other correctness or logic concerns not covered by the checklist above. The checklist is a starting point, not a boundary. Use your own reasoning to identify issues that fall outside the listed categories.

---

## Role 2: Security and Error Handling

**Assigned to: LLM 2 (External reviewer)**

This reviewer's job is to identify security vulnerabilities, improper error handling, and data safety issues. It should assume that all external input is untrusted.

### Review Checklist

For each item, respond with PASS, FAIL, or WARN and provide details for any non-PASS result.

**Input validation:**
- [ ] All user-provided input is validated before use (type, format, length, range).
- [ ] Input validation happens at the boundary (API endpoints, form handlers) before data enters the system.
- [ ] Validation failures produce clear error messages without exposing internal details.

**Injection prevention:**
- [ ] SQL queries use parameterized statements or prepared queries. No string concatenation of user input into queries.
- [ ] HTML output is properly escaped to prevent cross-site scripting (XSS).
- [ ] Command execution, if present, does not incorporate unsanitized user input.
- [ ] File paths constructed from user input are validated and restricted to expected directories.

**Authentication and authorization:**
- [ ] Protected routes and endpoints verify that the user is authenticated.
- [ ] Actions are authorized, meaning the authenticated user has permission to perform the requested operation.
- [ ] Authorization checks cannot be bypassed by modifying request parameters or headers.
- [ ] Session tokens and credentials are handled securely (not logged, not exposed in URLs, not stored in plain text).

**Data protection:**
- [ ] Sensitive data (passwords, tokens, personal information) is not written to logs.
- [ ] API responses do not include more data than the client needs.
- [ ] Secrets and configuration values are loaded from environment variables or secure stores, not hardcoded.
- [ ] Temporary files containing sensitive data are cleaned up after use.

**Error handling:**
- [ ] Errors are caught and handled at appropriate levels. No unhandled exceptions that could crash the application.
- [ ] Error messages returned to users are helpful without revealing internal implementation details, stack traces, or database structure.
- [ ] Failed operations are logged with enough detail to diagnose the problem.
- [ ] Error handling does not silently swallow errors in a way that hides bugs.

**Dependency security:**
- [ ] New dependencies added in this phase are from reputable, actively maintained sources.
- [ ] Dependencies are pinned to specific versions to prevent unexpected updates.
- [ ] All dependencies (new and existing) have been checked for known CVEs against their pinned versions. Validate against publicly available vulnerability databases such as the NIST National Vulnerability Database (nvd.nist.gov), MITRE CVE (cve.mitre.org), GitHub Advisory Database (github.com/advisories), or the relevant ecosystem's audit tooling (e.g., `npm audit`, `pip-audit`, `dotnet list package --vulnerable`, `mvn dependency-check:check` via OWASP Dependency-Check for Java/Maven, or the OWASP Gradle plugin for Gradle projects). Flag any dependency with an open CVE of medium severity or higher.
- [ ] Transitive dependencies (dependencies of your dependencies) have also been checked. A direct dependency may be clean while pulling in a vulnerable sub-dependency.

### Narrative Instructions

After completing the checklist, provide a written summary addressing:

1. Any attack vectors that the code introduces or fails to close, even if they seem unlikely.
2. Places where error handling exists but is insufficient, such as catching broad exception types or logging without alerting.
3. Any data exposure risks, even minor ones like verbose error messages in non-production configurations that could accidentally reach production.
4. Any other security or error handling concerns not covered by the checklist above. The checklist is a starting point, not a boundary. Use your own reasoning to identify risks that fall outside the listed categories.

---

## Role 3: Maintainability and Performance

**Assigned to: LLM 3 (Development LLM, fresh context)**

This reviewer's job is to evaluate whether the code is clean, well-structured, and performant. It should assess the code as if it will be maintained by a different developer six months from now.

### Review Checklist

For each item, respond with PASS, FAIL, or WARN and provide details for any non-PASS result.

**Code clarity:**
- [ ] Variable, function, and class names clearly communicate their purpose.
- [ ] Functions are focused on a single responsibility. No function does too many things.
- [ ] Complex logic is accompanied by comments that explain *why*, not just *what*.
- [ ] Magic numbers and hardcoded strings are replaced with named constants or configuration values.

**Structure and organization:**
- [ ] The code follows the patterns and conventions established in the existing codebase.
- [ ] New files are placed in logical locations within the project structure.
- [ ] Code duplication is minimal. Shared logic is extracted into reusable functions or modules.
- [ ] Imports and dependencies are organized consistently with the rest of the project.

**Architectural alignment:**
- [ ] The implementation respects the boundaries defined in the architecture document (separation of concerns, layer responsibilities).
- [ ] No shortcuts were taken that create tight coupling between components that should be independent.
- [ ] The code does not introduce patterns that conflict with the established architecture.

**UI consistency (if applicable):**
- [ ] New UI elements follow the same visual patterns, spacing, and styling conventions used elsewhere in the application.
- [ ] Interactive behaviors (loading states, error displays, empty states, transitions) are consistent with how similar interactions work in existing parts of the application.
- [ ] Labels, messages, and terminology are consistent with the rest of the UI. No conflicting names for the same concept across different screens.

**Performance:**
- [ ] Database queries are efficient. No N+1 query patterns, unnecessary joins, or missing indexes for common lookups.
- [ ] Large data sets are paginated or streamed rather than loaded entirely into memory.
- [ ] Expensive operations (network calls, file I/O, heavy computation) are not performed unnecessarily or redundantly.
- [ ] Caching is used where appropriate for data that is read frequently and changes infrequently.

**Testability:**
- [ ] The code is structured in a way that makes it easy to test. Dependencies can be mocked or injected.
- [ ] Unit tests included in this phase cover the key logic paths.
- [ ] Tests are clear about what they are verifying. Test names describe the scenario and expected outcome.
- [ ] Tests do not depend on external state or execution order.

**Technical debt:**
- [ ] No TODO comments without associated tracking (issue numbers, phase references).
- [ ] No commented-out code left in the codebase.
- [ ] No temporary workarounds without documentation explaining why they exist and when they should be removed.

### Narrative Instructions

After completing the checklist, provide a written summary addressing:

1. Areas where the code works correctly but could be simplified or made more readable.
2. Performance concerns that may not cause problems at current scale but could become bottlenecks as usage grows.
3. Any patterns in the code that a new developer would find confusing or that deviate from the conventions in the rest of the codebase without clear justification.
4. Any other maintainability, performance, or structural concerns not covered by the checklist above. The checklist is a starting point, not a boundary. Use your own reasoning to identify issues that fall outside the listed categories.

---

## Optional: Combined Review (Single LLM)

If you are using a single LLM for all three review roles (for smaller changes or when multiple models are not available), use the following prompt:

```
I need you to perform a comprehensive code review covering correctness, security, and maintainability.

Please review the following code changes against the phase document provided.

For each of the three areas, provide:
1. A summary checklist with pass/fail/warn status
2. Detailed narrative feedback for any non-pass items

Prioritize your findings by severity: security issues first, correctness issues second, maintainability issues third.

[Paste the phase document]
[Paste the diff or file contents]
```

When using a single LLM, be aware that the review will be less thorough than using three separate models with dedicated focus areas. The single-LLM approach is best suited for smaller, lower-risk changes.
