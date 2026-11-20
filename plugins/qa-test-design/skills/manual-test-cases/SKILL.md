---
name: manual-test-cases
description: Design structured manual test cases from a requirement, user story, acceptance criteria, Jira ticket or feature description. Use when asked to write test cases, a test plan for a feature, test scenarios, a checklist for manual testing, or to check coverage of existing test cases — even phrased as just "what should I test here?".
---

# Manual Test Cases

Turn a requirement into a complete, reviewable set of manual test cases.

## Inputs

Work from whatever the user gives: a user story, acceptance criteria, a Jira ticket, a spec, a screenshot or a plain description. If something that changes the test design is missing (roles/permissions, input limits, supported platforms), list it under **Open questions** instead of guessing — do not block on it.

## Process

1. **Understand the feature.** Restate in 2–4 bullets what the feature does, who uses it, and what is in and out of scope.
2. **Identify test conditions.** List everything that can be verified: each acceptance criterion, business rule, input field, state transition, role and integration point.
3. **Apply test design techniques.** For each condition pick the techniques that fit — see `references/test-design-techniques.md`:
   - Equivalence partitioning and boundary value analysis for inputs with ranges or formats
   - Decision tables for combinations of rules or conditions
   - State transition for workflows and statuses
   - Error guessing for known risk areas (empty, special characters, double-submit, session expiry, concurrency)
4. **Write the test cases** in the format from `references/test-case-template.md`. Cover, in order:
   - Positive (happy path) cases
   - Negative and validation cases
   - Boundary and edge cases
   - Role / permission cases, if roles exist
   - Non-functional checks worth doing manually (UI/UX, accessibility basics, responsiveness), only when relevant
5. **Build the traceability matrix** mapping each requirement / acceptance criterion to its test case IDs. Flag any requirement without a test.
6. **Review** against the checklist below before presenting.

## Writing rules

- One test case verifies one thing. Split cases whose expected result says "and also".
- Steps are concrete actions with concrete data ("Enter `abc@` in Email"), not "enter invalid email".
- Every step that changes something has an observable expected result.
- Preconditions state the user role, data state and environment needed — the test must be runnable by someone who did not write it.
- Titles follow `Verify <behavior> when <condition>`.
- Priority: **High** = core flow or data loss/security risk; **Medium** = important alternative flows and validation; **Low** = cosmetic and rare edge cases.

## Output

Default to a Markdown table per section (see the template). If the user asks for CSV, TestRail, Xray, Zephyr or Gherkin, use the matching format from `references/test-case-template.md`.

End with:
- **Traceability matrix**
- **Open questions / assumptions**
- **Coverage summary**: count of cases by type and priority

See `references/examples.md` for a complete worked example.

## Review checklist

- [ ] Every acceptance criterion has at least one positive and one negative case
- [ ] Boundaries tested at min−1, min, max, max+1 where limits exist
- [ ] No duplicate or overlapping cases
- [ ] Expected results are specific and verifiable
- [ ] Test data is concrete
- [ ] Assumptions are stated, not hidden in steps
