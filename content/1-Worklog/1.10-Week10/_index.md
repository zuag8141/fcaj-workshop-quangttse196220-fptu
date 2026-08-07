---
title: "Week 10 Worklog"
date: "2026-07-25"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Objectives for Week 10:

* Fix the "mark as paid" flow in the Settlement page.
* Fix the user avatar display issue in the Sidebar and Dashboard.
* Update the group-related types and services used by the Settlement page.
* Test the fixed flows and integrate the changes into the project through a pull request.

### Tasks to be carried out this week:

| Day | Task | Start date | Completion date | Reference materials |
| --- | --- | --- | --- | --- |
| Monday | - Review the Settlement page and reproduce the "mark as paid" issue. <br> - Analyze the settlement and group data flow on the frontend. <br> - Identify the root cause of the bug. | 20/07/2026 | 20/07/2026 | |
| Tuesday | - Fix the "mark as paid" flow in the Settlement page with the commit `fix-mark-as-paid`. <br>&emsp; + Correct the settlement status update logic. <br>&emsp; + Update the related group types and service calls. <br> - Create and merge Pull Request `#12`. | 21/07/2026 | 21/07/2026 | |
| Wednesday | - Fix the avatar display issue with the commit `fix-avatar`. <br>&emsp; + Update the Sidebar component. <br>&emsp; + Update the Dashboard page. <br> - Reapply the mark-as-paid and avatar fixes after a revert. | 22/07/2026 | 22/07/2026 | |
| Thursday | - Apply the final avatar fix with the commit `fix-avatar-2`. <br> - Test the mark-as-paid and avatar flows after the fixes. <br> - Verify the Settlement page and Dashboard work correctly. | 23/07/2026 | 23/07/2026 | |
| Friday | - Review the merged changes on the main branch. <br> - Run regression tests on the affected screens. <br> - Record the results and prepare the weekly report. | 24/07/2026 | 24/07/2026 | |

### Results achieved in Week 10:

* General results:
  * Fixed the "mark as paid" bug in the Settlement page.
  * Fixed the avatar display issue across the Sidebar and Dashboard.
  * Integrated the changes into the project through Pull Request `#12`.

* Completed fixes:
  * The Settlement page now correctly updates the "mark as paid" status.
  * The avatar is displayed correctly in the Sidebar and Dashboard.
  * Updated the group types and service logic used by the Settlement page.

* Knowledge and experience gained:
  * Improved my debugging skills for frontend state and data flow.
  * Gained more experience with Git: creating branches, commits, reverts, and pull requests.
