---
name: testcase-creator-skill
description: Create detailed, structured QA test cases from requirements, user stories, acceptance criteria, PRDs, screenshots, or feature descriptions. Use whenever the user asks to write, generate, review, or expand test cases, test scenarios, or a test suite for a UI, API, or workflow.
---

# Test Case Creator

Turn requirements into clear, executable, traceable test cases that any tester can run without asking questions.

## Workflow

1. **Understand the input.** Read the requirement, user story, acceptance criteria, PRD, API spec, or screenshot. Identify modules, user roles, inputs, outputs, business rules, and integrations.
2. **Identify gaps.** If anything critical is unclear (validation rules, limits, error messages, roles), do not guess. Make a reasonable assumption, state it, and list it under "Open Questions / Assumptions".
3. **Derive scenarios** using these techniques:
   - Equivalence partitioning (valid and invalid classes)
   - Boundary value analysis (min, max, min-1, max+1, empty, null)
   - Decision tables for combined business rules
   - State transitions for workflows (e.g., Draft → Submitted → Approved)
   - Error guessing for common failure points
4. **Write test cases** using the template below.
5. **Produce the coverage summary** and self-check against the Quality Checklist before returning output.

## Test Case Template

Every test case MUST follow this exact structure. Do not skip fields; write "N/A" if a field does not apply.

```
### TC-<MODULE>-<NNN>: <Short, action-oriented title>

| Field            | Value                                              |
|------------------|----------------------------------------------------|
| Test Case ID     | TC-<MODULE>-<NNN> (e.g., TC-LOGIN-001)             |
| Module / Feature | <Module name>                                      |
| Requirement ID   | <User story / requirement reference, e.g., US-102> |
| Test Type        | Functional / Negative / Boundary / UI / API / Security / Performance / Regression |
| Priority         | P1 (Critical) / P2 (High) / P3 (Medium) / P4 (Low) |
| Severity if fails| Blocker / Critical / Major / Minor / Trivial       |
| Automation       | Automatable / Manual only / Automated              |

**Description:** <One or two sentences on what this test verifies and why.>

**Preconditions:**
1. <System state, user role, environment, feature flags>
2. <Any data that must already exist>

**Test Data:**
- <field>: <value>
- <field>: <value>

**Test Steps:**

| # | Action                         | Expected Result                  |
|---|--------------------------------|----------------------------------|
| 1 | <Single, specific user action> | <Observable, verifiable outcome> |
| 2 | <Action>                       | <Expected result>                |

**Final Expected Result:** <Overall outcome that defines a pass.>

**Postconditions:** <State of the system after the test; any cleanup needed.>

**Actual Result:** _(to be filled during execution)_
**Status:** Not Run / Pass / Fail / Blocked / Skipped
**Executed By / Date:** _(to be filled)_
**Defect ID:** _(if failed)_
```

## API Test Case Variant

For API testing, replace the Test Steps table with:

```
**Request:**
- Method: GET / POST / PUT / PATCH / DELETE
- Endpoint: /api/v1/<resource>
- Headers: Content-Type: application/json; Authorization: Bearer <token>
- Body:
  { "field": "value" }

**Expected Response:**
- Status Code: 200 / 201 / 400 / 401 / 403 / 404 / 409 / 422 / 500
- Body / Schema: <key fields and values to assert>
- Headers: <any headers to assert>
- Response Time: < <N> ms
```

Always cover for APIs: valid request, missing required fields, invalid data types, missing/expired/invalid auth token, insufficient permissions, non-existent resource, duplicate creation, and payload size limits.

## Rules for Writing Test Cases

1. **One objective per test case.** If a title needs "and", split it.
2. **Steps are atomic.** One action per step, written as an imperative ("Click Submit", not "User should click Submit").
3. **Expected results are verifiable.** Use exact messages, values, URLs, or states. Never write "works correctly" or "as expected".
4. **Use concrete test data.** Real-looking values (e.g., `user@test.com`, `Passw0rd!`), never "valid email".
5. **Cover these categories for every feature:**
   - Positive (happy path)
   - Negative (invalid input, unauthorized access, missing fields)
   - Boundary values (min, max, min-1, max+1, empty, null)
   - Edge cases (special characters, very long input, whitespace, concurrency, timeouts, network loss)
6. **Consider non-functional checks where relevant:** security (SQL injection, XSS, session handling), accessibility (keyboard navigation, screen reader labels), cross-browser/device, and performance.
7. **Priority guidance:** P1 = core flow or data/security risk; P2 = major feature; P3 = secondary behavior; P4 = cosmetic.
8. **IDs are sequential per module** and never reused.
9. **Traceability.** Every test case links to a requirement ID. If none was given, use `REQ-<MODULE>-<NN>` and note it as an assumption.
10. **No duplicate tests.** Merge test cases that verify the same thing with the same data class.

## Output Format

1. **Coverage Summary** table first:

| Module | Positive | Negative | Boundary | Edge | Total |
|--------|----------|----------|----------|------|-------|

2. **Test cases** grouped by module, P1 first.
3. **Open Questions / Assumptions** at the end.

If the user asks for a spreadsheet (Excel/CSV/Google Sheets), use these columns, with each test step on its own row under the same Test Case ID:

`Test Case ID | Module | Requirement ID | Title | Test Type | Priority | Preconditions | Test Data | Step # | Action | Expected Result | Actual Result | Status | Defect ID`

If the user asks for Gherkin/BDD, convert each test case to:

```gherkin
Scenario: <Title>
  Given <precondition>
  When <action>
  Then <expected result>
```

## Quality Checklist (self-check before output)

- [ ] Every acceptance criterion is covered by at least one test case
- [ ] Positive, negative, boundary, and edge cases are all present
- [ ] No vague expected results ("works fine", "as expected", "proper error")
- [ ] Every step is a single action
- [ ] Test data is concrete
- [ ] IDs are unique and sequential
- [ ] Priorities are assigned with the guidance above
- [ ] Assumptions and open questions are listed

## Worked Example

**Input:** "Users can log in with email and password. Password must be 8–20 characters. Account locks after 5 failed attempts."

### TC-LOGIN-001: Log in with valid email and password

| Field            | Value                 |
|------------------|-----------------------|
| Test Case ID     | TC-LOGIN-001          |
| Module / Feature | Login                 |
| Requirement ID   | REQ-LOGIN-01          |
| Test Type        | Functional            |
| Priority         | P1 (Critical)         |
| Severity if fails| Blocker               |
| Automation       | Automatable           |

**Description:** Verifies a registered user can log in with correct credentials.

**Preconditions:**
1. User account `qa.user@test.com` exists and is active.
2. User is logged out and on the Login page.

**Test Data:**
- Email: `qa.user@test.com`
- Password: `Passw0rd!`

**Test Steps:**

| # | Action                                   | Expected Result                                  |
|---|------------------------------------------|--------------------------------------------------|
| 1 | Enter `qa.user@test.com` in Email field   | Email is displayed in the field                   |
| 2 | Enter `Passw0rd!` in Password field       | Password is masked                                |
| 3 | Click **Log in**                          | User is redirected to `/dashboard`                |
| 4 | Observe the header                        | "Welcome, QA User" and a Logout option are shown  |

**Final Expected Result:** User is logged in and lands on the dashboard.

**Postconditions:** Active session created; failed-attempt counter is 0.

**Actual Result:** _(to be filled)_
**Status:** Not Run

### TC-LOGIN-005: Account locks after 5 consecutive failed attempts

| Field            | Value                 |
|------------------|-----------------------|
| Test Case ID     | TC-LOGIN-005          |
| Module / Feature | Login                 |
| Requirement ID   | REQ-LOGIN-03          |
| Test Type        | Security              |
| Priority         | P1 (Critical)         |
| Severity if fails| Critical              |
| Automation       | Automatable           |

**Description:** Verifies the account is locked after the 5th consecutive wrong password.

**Preconditions:**
1. Account `qa.user@test.com` is active with 0 failed attempts.

**Test Data:**
- Email: `qa.user@test.com`
- Wrong password: `WrongPass1`
- Correct password: `Passw0rd!`

**Test Steps:**

| # | Action                                              | Expected Result                                         |
|---|-----------------------------------------------------|---------------------------------------------------------|
| 1 | Log in with `WrongPass1` 4 times                     | "Invalid email or password" shown each time; not locked  |
| 2 | Log in with `WrongPass1` a 5th time                  | Message: "Your account is locked. Try again later or reset your password." |
| 3 | Log in with correct password `Passw0rd!`             | Login is blocked; lock message is still shown            |

**Final Expected Result:** Account is locked after 5 failed attempts, and correct credentials are rejected while locked.

**Postconditions:** Unlock the account via admin or reset flow before other tests.

**Actual Result:** _(to be filled)_
**Status:** Not Run

**Other cases for this example** (write fully in real output): password of 7, 8, 20, and 21 characters (boundary); empty email/password; invalid email format; unregistered email; SQL injection in email field; counter resets after a successful login; lock message does not reveal whether the email exists.

**Open Questions / Assumptions:**
- Lock duration is not specified; assumed manual unlock or reset.
- Exact error message text assumed; confirm with product.






-------------
name:	test-case-creator
description:	Based on my template, which I am going to give him, it is going to create a test case according to my company. VWO.com
version:	1.0.0
author:	Padmasri PML
license:	MIT
tags: testcase	

name	test-case-writer
description	Expand approved test scenarios into detailed, executable test cases. Use when a tester says "write test cases for TS-1", "detail these scenarios", or hands over a reviewed scenario list ready to become step-by-step cases. Produces preconditions, ordered steps, expected results, and required data for each case, every one traceable to its scenario and acceptance criterion, then stops for review before the cases are treated as final.
license	MIT
metadata	
author	stlc-phase	version
TheTestingAcademy
Test Case Development
1.0.0
Test Case Writer

You convert approved scenarios into cases a human (or automation) can run without guessing. Precision and repeatability are the whole point — no ambiguous steps.

When to use

Scenarios have been designed and reviewed, and now need executable detail.
Someone points at a scenario ID and asks for the full step-by-step case.
A regression pack needs cases written from an agreed coverage set.
Workflow

Confirm the source is approved. Work from reviewed scenarios, not raw requirements. If a scenario is unclear or unapproved, ask before expanding it — do not improvise coverage.
Write each case. For every scenario produce: a unique ID, title, preconditions, test data references, numbered steps (one action per step), and a specific expected result per step. Keep steps atomic and observable.
Cover the variants. Positive, negative, and boundary paths each get their own case; do not collapse them into one multi-branch case.
Trace. Every case cites its scenario ID and the AC it verifies. Flag any scenario with no case and any case that maps to nothing.
HUMAN REVIEW GATE (mandatory). Present the cases as a draft. List assumptions about data, environment, and expected results you could not confirm. Ask a reviewer to approve or edit before the cases enter the suite or feed automation.
Output shape

## Test Cases — <feature / JIRA-KEY>
TC-1  <title>            (from TS-1, AC-1)   Priority: P0
  Preconditions: ...
  Test data: <ref to data set>
  Steps:
    1. <action>            -> Expected: <observable result>
    2. <action>            -> Expected: <observable result>
  Postconditions / cleanup: ...
--- HUMAN REVIEW GATE ---
Assumptions / Unconfirmed expected results / "Approve or edit before these go live"
Guardrails

Never invent an expected result you cannot justify from an AC or the scenario — mark it "expected result to confirm" instead of guessing.
Do not fabricate concrete data values inline; reference the data set from the data generator.
Keep steps deterministic and observable — no "verify it works" hand-waving.
Cases are a draft until a human approves; you do not sign off your own cases.