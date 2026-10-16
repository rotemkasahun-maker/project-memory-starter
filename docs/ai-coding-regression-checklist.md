# A practical regression checklist for AI-generated code

An AI coding assistant can implement a feature correctly and still break an unrelated workflow. The new button may work, while an existing form stops saving, a shared permission check changes, or a mobile screen becomes unusable.

The question before merging is not simply, “Did the assistant finish?” It is, “What working behavior might this change have disturbed, and how can I check?”

This checklist is designed for a small change in a real app. It doesn't require a comprehensive test suite, but it does require evidence instead of reassuring summaries.

## 1. State the change and its blast radius

Describe the new user-facing behavior in one sentence. Then list the systems it touches.

Suppose you're adding a **Duplicate project** button.

**New behavior:** a signed-in project owner can create a separate copy of an existing project.

**Possible blast radius:** project creation, list ordering, ownership checks, quota limits, project detail routes, shared objects and timestamps.

A list of modified files isn't enough. A change to a shared helper can affect routes that the assistant never edited.

Ask for three separate lists: new behavior, existing behavior at risk and explicitly out-of-scope behavior. This makes it easier to spot accidental scope expansion.

## 2. Establish a baseline before changing anything

Choose a few checks that represent behavior you already rely on.

For the duplicate-project example:

- The owner can still create a normal project.
- Another user cannot see or duplicate the owner's project.
- Editing the copy does not alter the original.
- Signed-out requests are rejected.
- Project lists retain the expected ownership and ordering.

Run these before the change when possible. If one already fails, record it as a pre-existing defect. If you cannot run a baseline, write **NOT RUN**, not PASS.

This distinction matters when you're reviewing a bug later. A failure observed after a change is not automatically caused by that change.

## 3. Test five distinct kinds of behavior

**A. New happy path.** Complete the promised job using realistic data. Inspect the saved result, not just a success message.

**B. Adjacent existing path.** Exercise the closest create, edit, delete or read workflow that shares the changed code.

**C. Permissions and isolation.** Try a second account and a signed-out session. Hiding a button in the interface does not enforce authorization; the server must reject unauthorized access.

**D. Failure states.** Try empty input, duplicate submission, invalid IDs, slow requests and expired sessions. Confirm the app reports accurately what was saved and what failed.

**E. Mobile and navigation.** Check a narrow viewport, keyboard focus, direct-page reload, browser back and deep links where applicable.

You don't need the same depth for every change. A text-color tweak calls for proportionate checks. Changes to authentication, payments, uploads or tenant isolation need much stronger coverage.

## 4. Give the assistant a testable assignment

“Check everything” is too broad and makes unsupported green checkmarks easy.

Try this instead:

> Review this change as a regression tester. Do not edit code yet. Describe the user-facing promise, shared dependencies and highest-risk existing workflows. Propose a small test matrix covering the new path, adjacent behavior, authorization, failure states and mobile/navigation where relevant. Separate checks actually executed from checks merely proposed. For each failure, provide reproduction steps, expected behavior, actual behavior and evidence. Never mark PASS without execution evidence.

The output should tell you what to run and what remains uncertain. A suggested test is not a completed test.

## 5. Keep a small regression record

Use a reusable record in your repository:

```text
CHANGE ID:
User-facing promise:
Candidate commit/build:
Changed components:
Existing workflows at risk:
Test environment:
Synthetic accounts/data:

TEST                     EXPECTED        ACTUAL       RESULT       EVIDENCE
New core flow            ...             ...          NOT RUN      ...
Existing adjacent flow   ...             ...          NOT RUN      ...
Second-user access       Denied          ...          NOT RUN      ...
Failure/empty state      Clear recovery  ...          NOT RUN      ...
Mobile/direct reload     Usable          ...          NOT RUN      ...

Known baseline failures:
New defects:
Release blockers:
Reviewer/date:
Decision: MERGE / FIX THEN RETEST / NEEDS EVIDENCE
```

Keep evidence links short and reproducible. Avoid pasting real customer records, passwords or API keys into AI chats or public issue trackers.

## 6. Reproduce one defect before fixing it

Imagine that duplicating a project appears to work, but editing the duplicate changes the original project's due date.

**Reproduction:** Create Project A with a due date of November 12. Duplicate it. Change the duplicate's date to November 20. Reopen A.

**Expected:** A still shows November 12.

**Actual:** A shows November 20.

**Possible cause:** a shared mutable object or update path. That's a hypothesis, not a confirmed root cause.

Where practical, add a failing regression test that asserts the original remains unchanged. Fix the smallest cause, then rerun the new test and the adjacent project-edit checks.

A plausible explanation is not a substitute for reproduction.

## 7. Check what automated tests may miss

Tests can pass while a first-time user still cannot understand the page.

Open the actual candidate build with fresh sample data. Find the new feature without knowing its internal name. Complete the job, trigger one error and see whether the interface explains what happened.

For a web interface, you can supplement this review with a visual comparison against a known-good page. **Don't Break My App**, our Chrome extension in public beta, is designed to flag possible page regressions for review. It does not replace authorization, persistence, accessibility, security or end-to-end testing.

[View Don't Break My App on the Chrome Web Store](https://chromewebstore.google.com/detail/dont-break-my-app/abphemiaaddgplkgdblajpkhblpgbngo?utm_source=github&utm_medium=organic_repo&utm_campaign=content_distribution_20261016&utm_content=regression_checklist_beta)

## 8. Make a decision based on evidence

**MERGE:** The promised workflow works, important adjacent behavior is verified and no release-blocking defect remains.

**FIX THEN RETEST:** A reproducible defect affects data integrity, access control or a critical existing flow.

**NEEDS EVIDENCE:** The build could not be run, a relevant account boundary was not tested or the review consisted only of code inspection.

For high-risk changes, an independent reviewer is useful. For a small change, a focused self-review can be sufficient if the evidence is still clear.

## 9. Leave the next session a useful handoff

Save the regression test or reproduction steps with the code. Record the exact commit or build, what was tested, what wasn't, known failures and the next safe action.

Without that record, the next assistant may repeat the same investigation and miss the behavior that must stay intact.

## Six questions before you merge

1. What user job changed?
2. Which existing workflow shares the changed code or state?
3. Did I test a realistic happy path and inspect persisted results?
4. Did I test the most important failure or permission boundary?
5. Did I look at the actual build as a user?
6. Could another developer reproduce my verdict from the evidence?

If your answer is “I assume so,” the next action is a targeted check, not a green status.

**Related free resource:** [Project Memory Starter](../README.md).

*Disclosure: We make Don't Break My App, Project Memory Starter and the paid Project Memory + QA Lite kit. The examples in this article are illustrative, not claims about real customer incidents.*

**Optional paid companion:** [Project Memory + QA Lite](https://rotemka.gumroad.com/l/project-memory-qa-lite?utm_source=github&utm_medium=organic_repo&utm_campaign=content_distribution_20261016&utm_content=regression_checklist_paid) adds decision, lesson, regression and release records to the free workflow. It is a manually maintained Markdown kit, not an automatic test runner. Check the current price and license on Gumroad; the checklist above does not require a purchase.
