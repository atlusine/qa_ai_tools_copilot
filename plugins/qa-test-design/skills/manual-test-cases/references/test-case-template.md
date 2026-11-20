# Test Case Templates

## Default: Markdown table

| ID | Title | Priority | Type | Preconditions | Steps | Test Data | Expected Result |
|----|-------|----------|------|---------------|-------|-----------|-----------------|
| TC-001 | Verify ... when ... | High | Positive | ... | 1. ...<br>2. ... | ... | ... |

- **ID**: `TC-<NNN>`, or `<FEATURE>-TC-<NNN>` when the user gives a feature key.
- **Type**: Positive, Negative, Boundary, Permission, UI, Accessibility, Integration.

## Detailed (single case)

```
ID:             TC-001
Title:          Verify <behavior> when <condition>
Requirement:    AC-1 / JIRA-123
Priority:       High | Medium | Low
Type:           Positive | Negative | Boundary | ...
Preconditions:  - ...
Test Data:      - ...
Steps:
  1. ...
  2. ...
Expected Result:
  1. ...
  2. ...
Postconditions: ...
```

## CSV (TestRail / Xray / Zephyr import)

```csv
ID,Title,Priority,Type,Preconditions,Steps,Expected Result,Requirement
TC-001,"Verify ... when ...",High,Positive,"...","1. ...\n2. ...","...",AC-1
```

Quote every text field; separate steps with `\n` inside the quoted cell.

## Gherkin

```gherkin
Feature: <feature name>

  Scenario: <title without "Verify">
    Given <precondition>
    When <action>
    Then <expected result>
```

Use `Scenario Outline` with `Examples` when cases differ only in data.

## Traceability matrix

| Requirement | Description | Test Cases | Covered |
|-------------|-------------|------------|---------|
| AC-1 | ... | TC-001, TC-004 | ✅ |
| AC-2 | ... | — | ❌ |
