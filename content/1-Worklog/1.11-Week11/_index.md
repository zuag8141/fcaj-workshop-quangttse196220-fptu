---
title: "Week 11 Worklog"
date: "2026-08-01"
weight: 1
chapter: false
pre: " <b> 1.11. </b> "
---

### Objectives for Week 11:

* Develop and complete the project's notification system.
* Build APIs for managing the notification inbox and user notification preferences.
* Add notifications for activities related to groups, expenses, settlements, and product updates.
* Implement the complaint submission flow for users.
* Implement the complaint review and handling flow for administrators.
* Test, synchronize, and integrate source code changes through pull requests.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Start date | Completion date | Reference materials |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------- |
| Monday | - Analyze the requirements of the notification system. <br> - Identify the types of notifications required by the project. <br> - Review notification flows related to groups, expenses, and settlements. <br> - Review the notification module structure and prepare the required APIs.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | 27/07/2026 | 27/07/2026      |                     |
| Tuesday | - Implement notification preference and inbox management with the commit `feat: add notification preferences and inbox APIs`. <br>&emsp; + Add APIs for managing notification preferences. <br>&emsp; + Add an API for retrieving the user's notification list. <br>&emsp; + Support the management of read and unread notification statuses. <br> - Implement the debtor payment flow with the commit `feat: add debtor settlement and payment sent flow`. <br>&emsp; + Allow debtors to mark payments as sent. <br>&emsp; + Generate notifications related to payment submission and settlement confirmation. <br> - Integrate the changes through Pull Request `#14`.                                                                                                                        | 28/07/2026 | 28/07/2026      |                     |
| Wednesday | - Add expense and group membership notifications with the commit `feat: add expense and group membership notifications`. <br>&emsp; + Generate notifications for activities related to expenses. <br>&emsp; + Generate notifications when members are added or when group membership changes occur. <br> - Add weekly settlement reminder notifications with the commit `feat: add weekly on-demand settlement reminder notifications`. <br>&emsp; + Support the creation of on-demand settlement reminders. <br>&emsp; + Add a weekly recurring settlement reminder mechanism.                                                                                                                                                                                                                 | 29/07/2026 | 29/07/2026      |                     |
| Thursday | - Finalize the notification system with the commit `feat: add product update notifications and finalize notification APIs`. <br>&emsp; + Add product update notifications. <br>&emsp; + Complete the remaining APIs in the notification module. <br>&emsp; + Review API responses, validations, and access permissions. <br> - Integrate the changes through Pull Request `#15`. <br> - Verify the notification inbox and notification preferences after merging.                                                                                                                                                                                                                                                                                                                               | 30/07/2026 | 30/07/2026      |                     |
| Friday | - Implement the user complaint submission feature with the commit `feat(complaint): implement user complaint submission feature`. <br>&emsp; + Allow users to create and submit complaints. <br>&emsp; + Store complaint information and associate it with the submitting user. <br>&emsp; + Validate input data and complaint submission permissions. <br> - Implement the administrator complaint handling flow with the commit `feat(admin): implement complaint handling flow`. <br>&emsp; + Allow administrators to view complaints that require review. <br>&emsp; + Support updating complaint results and processing statuses. <br>&emsp; + Add authorization checks for administrator actions. <br> - Test the complete complaint flow from user submission to administrator handling. | 31/07/2026 | 31/07/2026      |                     |

### Results achieved in Week 11:

* General results:
  * This week, I focused on developing the backend notification system and complaint management features.
  * The notification system was expanded to support activities such as group membership changes, expense creation, payment submission, settlement reminders, and product updates.
  * I also implemented a complete complaint flow covering both sides: users submitting complaints and administrators reviewing and handling them.
  * Changes to the notification module were integrated into the project through Pull Requests `#14` and `#15`.

* Completed notification features:
  * Added APIs for managing user notification preferences.
  * Added APIs for managing the notification inbox.
  * Supported retrieving notifications and managing their read and unread statuses.
  * Added a flow that allows debtors to mark payments as sent.
  * Generated notifications for settlement and payment-sent activities.
  * Generated notifications for new expenses and other expense-related activities.
  * Generated notifications for group membership activities.
  * Added on-demand settlement reminder notifications.
  * Added weekly recurring settlement reminder notifications.
  * Added product update notifications.
  * Completed the main APIs of the notification module.

* Completed complaint features:
  * Allowed users to submit complaints through the system.
  * Validated and stored complaint information.
  * Associated each complaint with the user who submitted it.
  * Implemented a flow for administrators to view and handle complaints.
  * Allowed administrators to update complaint results and processing statuses.
  * Added administrator authorization checks for complaint handling actions.

* Contributed commits:
  * `feat: add notification preferences and inbox APIs`
  * `feat: add debtor settlement and payment sent flow`
  * `feat: add expense and group membership notifications`
  * `feat: add weekly on-demand settlement reminder notifications`
  * `feat: add product update notifications and finalize notification APIs`
  * `feat(complaint): implement user complaint submission feature`
  * `feat(admin): implement complaint handling flow`

* Merged pull requests:
  * Pull Request `#14`: Integrated notification preferences, inbox APIs, and the debtor settlement/payment-sent flow.
  * Pull Request `#15`: Integrated notifications for expenses, group membership, settlement reminders, and product updates, while also finalizing the notification APIs.

* Knowledge and experience gained:
  * Gained a better understanding of how to design a notification module for different types of system events.
  * Gained experience in developing APIs for notification inboxes and user notification preferences.
  * Learned how to integrate notifications with business functions such as groups, expenses, and settlements.
  * Gained experience in implementing recurring features such as weekly settlement reminders.
  * Understood how to design a complaint-handling workflow between users and administrators.
  * Improved my skills in authorization validation and backend status transitions.
  * Practiced the development workflow involving commits, pull requests, testing, and source code integration.
