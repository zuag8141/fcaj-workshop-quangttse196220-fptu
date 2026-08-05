---
title: "Week 10 Worklog"
date: "2026-07-25"
weight: 1
chapter: false
pre: " <b> 1.10. </b> "
---

### Objectives for Week 10:

* Participate in the development and improvement of the project's backend functions.
* Adjust the payment confirmation logic to ensure that user permissions are handled correctly.
* Separate notification preferences into an independent module to improve code maintainability and scalability.
* Add a member limit for groups using the free plan.
* Fix bugs, synchronize the source code with the `main` branch, and create pull requests to integrate the changes into the project.

### Tasks to be carried out this week:

| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Start date | Completion date | Reference materials |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------- |
| Monday    | - Review the backend source code and analyze the development requirements for the week. <br> - Analyze the expense settlement confirmation flow. <br> - Identify the permissions of debtors and creditors during the settlement confirmation process. <br> - Prepare a separate development branch for implementing and testing the changes.                                                                                                                                                                                                                                                                                                                                                                                                                                  | 20/07/2026 | 20/07/2026      |                     |
| Tuesday   | - Update the settlement confirmation logic with the commit `fix(expenses): restrict settlement confirmation to creditor`. <br>&emsp; + Restrict settlement confirmation permission to the creditor. <br>&emsp; + Prevent unauthorized users from confirming a payment. <br> - Create and merge Pull Request `#8` into the project. <br> - Refactor the notification function with the commit `refactor(notifications): extract notification preferences into separate module`. <br>&emsp; + Extract notification preferences into a separate module. <br>&emsp; + Reduce dependencies between components in the notification module. <br>&emsp; + Improve the maintainability and scalability of the source code. <br> - Create and merge Pull Request `#9` into the project. | 21/07/2026 | 21/07/2026      |                     |
| Wednesday | - Add a member limit for free-plan groups with the commit `feat(groups): limit free plan groups to 5 members`. <br>&emsp; + Check the group's subscription plan before adding a new member. <br>&emsp; + Limit groups using the free plan to a maximum of 5 members. <br>&emsp; + Return an appropriate error when the number of members exceeds the limit. <br> - Create and merge Pull Request `#10` into the project. <br> - Fix issues discovered during development with the commit `fixed bug`. <br> - Merge the `main` branch into the `be-Minh` branch to update the latest source code and resolve differences between branches. <br> - Create and merge Pull Request `#11` into the project.                                                                        | 22/07/2026 | 22/07/2026      |                     |
| Thursday  | - Review the changes after they were merged into the main branch. <br> - Test the cases related to settlement confirmation permissions. <br> - Test the member limit for groups using the free plan. <br> - Verify that notification preferences continue to work correctly after being extracted into a separate module. <br> - Check for issues that may have occurred during the source code merge process.                                                                                                                                                                                                                                                                                                                                                                | 23/07/2026 | 23/07/2026      |                     |
| Friday    | - Summarize the commits and pull requests contributed during the week. <br> - Check the merge status of Pull Requests `#8`, `#9`, `#10`, and `#11`. <br> - Review the source code and ensure that all completed functions were successfully integrated into the project. <br> - Record the results and prepare the weekly report.                                                                                                                                                                                                                                                                                                                                                                                                                                             | 24/07/2026 | 24/07/2026      |                     |

### Results achieved in Week 10:

* General results:
  * This week, I participated in backend development and contributed changes related to expense settlements, notification preferences, and the member limit for free-plan groups.
  * I completed the assigned functions, fixed bugs, synchronized the source code, and created pull requests to integrate the changes into the project's main branch.
  * A total of 4 pull requests were merged, including Pull Requests `#8`, `#9`, `#10`, and `#11`.

* Completed functions:
  * Updated the payment confirmation permissions so that only the creditor can confirm a settlement.
  * Prevented users who are not the creditor from performing the payment confirmation action.
  * Extracted notification preferences into a separate module.
  * Improved the source code structure of the notification function.
  * Added a business rule that limits groups using the free plan to a maximum of 5 members.
  * Added error handling for attempts to add members beyond the free-plan limit.
  * Fixed issues discovered during the development and integration processes.
  * Synchronized the `be-Minh` development branch with the `main` branch.

* Contributed commits:
  * `fix(expenses): restrict settlement confirmation to creditor`
  * `refactor(notifications): extract notification preferences into separate module`
  * `feat(groups): limit free plan groups to 5 members`
  * `fixed bug`
  * `Merge branch 'main' into be-Minh`

* Merged pull requests:
  * Pull Request `#8`: Integrated the change that restricts settlement confirmation to the creditor.
  * Pull Request `#9`: Integrated the refactoring of notification preferences.
  * Pull Request `#10`: Integrated the feature that limits free-plan groups to a maximum of 5 members.
  * Pull Request `#11`: Integrated bug fixes and source code synchronization changes.

* Knowledge and experience gained:
  * Gained a better understanding of how to implement business rules and validate user permissions in the backend.
  * Learned how to organize and separate modules to improve source code maintainability.
  * Understood how to restrict system functions based on a user's subscription plan.
  * Practiced the Git workflow, including creating commits, updating branches, merging branches, and managing pull requests.
  * Gained more experience in reviewing and testing functions after the source code was merged into the main branch.
