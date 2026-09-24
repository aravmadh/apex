# Lab 6: Review APEXToGo Patterns

## Introduction

Review the instructor-provided APEXToGo reference app. Identify patterns that would make ESS easier to use.

This is an observation activity. Do not copy its assets, code, or content without source and design approval.

Estimated Time: 4 minutes

### Objectives

- Compare an instructor-provided mobile reference app with ESS.
- Identify mobile patterns that fit the employee self-service use case.
- Turn observations into small, testable ESS improvements.

## Task 1: Compare the employee journeys

1. Open APEXToGo only when the instructor provides it. Otherwise, use the ESS pages and the worksheet below.

    This workshop does not install or package APEXToGo.

2. Compare the reference app with ESS. Observe only the behavior that is available in the supplied version.

    | Reference-app area. | ESS equivalent | What to evaluate |
    | --- | --- | --- |
    | Task list. | My Tasks Interactive Grid | Scanability, touch targets, and task completion flow. |
    | Profile. | My Profile form | Field grouping, edit flow, and sharing boundaries. |
    | Navigation. | ESS navigation | Check access to frequent actions. |
    | Attachments. | ESS onboarding uploads | Check camera or device file selection. |
    | Offline behavior. | Installed ESS PWA | Check the employee experience without network access. |

3. Record two or three patterns that fit ESS. Favor changes that remove friction from My Tasks, Leave Request, onboarding, or profile updates.

    Do not make a cosmetic copy of a mobile pattern.

## Task 2: Select a safe next improvement

1. Write a small acceptance test for each pattern. For example: “At 375 pixels wide, an employee completes a My Tasks item without horizontal scrolling.”

2. Use native APEX components and responsive CSS first. Add custom JavaScript only for a tested behavior that native components cannot provide.

3. Review changes with the ESS design and accessibility owners. Pay special attention to gestures, camera access, offline handling, and navigation.

## Acknowledgements

- **Author -** Aravind Madhavan, Senior Product Manager.
- **Last Updated By/Date** - Aravind Madhavan, Senior Product Manager, September 2026.
