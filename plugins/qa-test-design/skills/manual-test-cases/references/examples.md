# Worked Example

## Input

> **User story:** As a registered user, I want to reset my password via email so I can regain access to my account.
>
> **Acceptance criteria**
> - AC-1: "Forgot password?" link on the login page opens the reset form.
> - AC-2: Entering a registered email sends a reset link; the page shows a generic confirmation message.
> - AC-3: Entering an unregistered email shows the same generic message (no account enumeration).
> - AC-4: The reset link expires after 30 minutes and can be used only once.
> - AC-5: New password must be 8–64 characters with at least one digit.

## Feature summary

- Self-service password reset via emailed one-time link
- Users: registered, unauthenticated users
- Out of scope: SSO accounts, admin-initiated resets

## Test cases

### Positive

| ID | Title | Priority | Type | Preconditions | Steps | Test Data | Expected Result |
|----|-------|----------|------|---------------|-------|-----------|-----------------|
| TC-001 | Verify reset form opens when user clicks "Forgot password?" | High | Positive | Logged out, on login page | 1. Click "Forgot password?" | — | Reset form with Email field and Submit button is shown |
| TC-002 | Verify reset email is sent when a registered email is submitted | High | Positive | User `qa.user@test.com` exists | 1. Open reset form<br>2. Enter email<br>3. Click Submit | `qa.user@test.com` | Generic confirmation shown; reset email received within 2 min |
| TC-003 | Verify password is changed when a valid new password is set via the link | High | Positive | Reset email from TC-002 | 1. Open link<br>2. Enter new password twice<br>3. Submit<br>4. Log in with new password | `NewPass123` | Success message; login with new password succeeds; old password rejected |

### Negative

| ID | Title | Priority | Type | Preconditions | Steps | Test Data | Expected Result |
|----|-------|----------|------|---------------|-------|-----------|-----------------|
| TC-004 | Verify generic message is shown when an unregistered email is submitted | High | Negative | Email not registered | 1. Open reset form<br>2. Enter email<br>3. Submit | `nobody@test.com` | Same message and response time as TC-002; no email sent |
| TC-005 | Verify validation error when email format is invalid | Medium | Negative | On reset form | 1. Enter email<br>2. Submit | `abc@` | Inline error "Enter a valid email"; no request sent |
| TC-006 | Verify reset link is rejected when used a second time | High | Negative | Link already used in TC-003 | 1. Open the same link again | — | "Link is invalid or expired" page; password unchanged |
| TC-007 | Verify reset link is rejected after 30 minutes | High | Negative | Reset email requested | 1. Wait 31 min<br>2. Open link | — | "Link is invalid or expired" page |

### Boundary

| ID | Title | Priority | Type | Preconditions | Steps | Test Data | Expected Result |
|----|-------|----------|------|---------------|-------|-----------|-----------------|
| TC-008 | Verify password is rejected when it has 7 characters | Medium | Boundary | Valid reset link | 1. Enter password<br>2. Submit | `Abcde12` | Error "8–64 characters" |
| TC-009 | Verify password is accepted when it has 8 characters | Medium | Boundary | Valid reset link | 1. Enter password<br>2. Submit | `Abcdef12` | Password updated |
| TC-010 | Verify password is accepted when it has 64 characters | Medium | Boundary | Valid reset link | 1. Enter password<br>2. Submit | 63 × `a` + `1` | Password updated |
| TC-011 | Verify password is rejected when it has 65 characters | Medium | Boundary | Valid reset link | 1. Enter password<br>2. Submit | 64 × `a` + `1` | Error "8–64 characters" |
| TC-012 | Verify password is rejected when it has no digit | Medium | Negative | Valid reset link | 1. Enter password<br>2. Submit | `Abcdefgh` | Error "Must contain at least one digit" |
| TC-013 | Verify link still works at 29 minutes | Low | Boundary | Reset email requested | 1. Wait 29 min<br>2. Open link | — | Reset form opens |

## Traceability matrix

| Requirement | Test Cases | Covered |
|-------------|------------|---------|
| AC-1 | TC-001 | ✅ |
| AC-2 | TC-002, TC-005 | ✅ |
| AC-3 | TC-004 | ✅ |
| AC-4 | TC-006, TC-007, TC-013 | ✅ |
| AC-5 | TC-003, TC-008–TC-012 | ✅ |

## Open questions / assumptions

- Does requesting a second reset link invalidate the first? (Assumed yes — add a case once confirmed.)
- Is there rate limiting on reset requests?
- Are active sessions logged out after a password change?

## Coverage summary

| Type | High | Medium | Low | Total |
|------|------|--------|-----|-------|
| Positive | 3 | 0 | 0 | 3 |
| Negative | 3 | 2 | 0 | 5 |
| Boundary | 0 | 4 | 1 | 5 |
| **Total** | 6 | 6 | 1 | 13 |
