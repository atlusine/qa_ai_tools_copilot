# Test Design Techniques

## Equivalence Partitioning (EP)

Split inputs into classes the system should treat the same way; test one value per class.

*Age field, valid 18–65:* invalid `< 18`, valid `18–65`, invalid `> 65`, invalid non-numeric, empty.

Use for: any input with a range, format or category.

## Boundary Value Analysis (BVA)

Defects cluster at edges. For a range `min..max` test `min−1, min, max, max+1` (add `min+1, max−1` for high-risk fields).

*Password 8–64 chars:* 7, 8, 64, 65.

Also apply to: string length, file size, dates (start/end of month, leap year), counts (0, 1, many), pagination (last item on page, first on next).

## Decision Table

List conditions as columns, combinations as rules, and the expected action per rule. Collapse rules where a condition doesn't matter.

| Rule | Logged in | Has permission | Item exists | → Result |
|------|-----------|----------------|-------------|----------|
| R1 | N | – | – | Redirect to login |
| R2 | Y | N | – | 403 message |
| R3 | Y | Y | N | 404 message |
| R4 | Y | Y | Y | Item shown |

Use for: business rules with several interacting conditions.

## State Transition

Draw states and the events that move between them. Test every valid transition and the important invalid ones.

*Ticket:* Draft → Submitted → In Review → Approved / Rejected → (Rejected) → Draft.
Invalid: Draft → Approved directly; editing an Approved ticket.

Use for: workflows, statuses, wizards, sessions.

## Pairwise / Combinatorial

When many parameters each have several values, cover every *pair* of values instead of every combination.

Use for: browser × OS × role × locale matrices.

## Error Guessing (risk checklist)

- Empty, whitespace-only, very long input
- Special characters, emoji, HTML/script tags, SQL-like strings
- Leading/trailing spaces, mixed case
- Double click / double submit, back button after submit
- Refresh mid-flow, session timeout, multiple tabs
- Concurrent edits by two users
- Slow or lost network, API error responses
- Timezones, locale date/number formats
- Permissions changed while the user is logged in
- Deleted or archived referenced data
