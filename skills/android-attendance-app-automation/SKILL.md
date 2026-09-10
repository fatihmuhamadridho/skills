---
name: android-attendance-app-automation
description: Automate Android attendance-app clock-in and clock-out flows with selector-backed inspection, guarded submission, and result verification.
---

# Android Attendance App Automation

Use for Android attendance workflows: open attendance app, inspect clock-in or
clock-out detail, submit when authorized, and verify result.

This skill depends on android-remote for device readiness, wake/unlock handling,
selector-first interaction, ADB verification, and guarded actions.

## Required app context

Obtain package name and launcher activity from user context, installed-app
inspection, or verified foreground state. Never hardcode organization-specific app
identifiers. Reuse verified identifiers during same session.

## Verification checkpoints

Keep these states separate:

- command delivered
- target app verified foreground
- home Clock In or Clock Out control visible
- matching detail page visible
- matching detail submit control visible
- submit control actually tapped
- post-submit success verified

Do not claim later checkpoint from earlier one.

## Core workflow

1. Use android-remote to verify target device, connection, wake, and unlock.
2. Resolve target app package and launcher activity.
3. Verify foreground app; launch target when needed and re-check focus.
4. Inspect selectors before coordinates.
5. Verify target home control before tapping.
6. Verify detail page using matching header, attendance information, and matching
   submit control.
7. Stop before final submit unless user explicitly requested direct submission.
8. After submission, verify toast, changed attendance status, success page, or other
   clear app-local signal.
9. If final signal is unavailable, report submission as unverified.

## Clock In

Verify home Clock In, open detail, verify Clock In header and submit control, then
submit only when authorized. Verify final state before reporting success.

## Clock Out

Verify home Clock Out, open detail, verify Clock Out header and submit control, then
submit only when authorized. Verify final state before reporting success.

## Inspection order

1. ADB foreground check
2. direct UIAutomator2 selector lookup
3. UIAutomator2 hierarchy dump
4. adb uiautomator dump when stable
5. coordinates only for low-risk navigation after evidence review

If hierarchy target is missing, refresh once, re-check foreground, inspect once more,
then stop with blocker. Never stack coordinate guesses for state-changing actions.

## Reporting

Report device serial, verified package/activity, foreground evidence, inspection
layer, verified controls/pages, whether submit occurred, and whether final success
was verified.
