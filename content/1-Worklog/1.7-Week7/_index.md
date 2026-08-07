---
title: "Week 7 Worklog"
date: "2026-07-10"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Objectives for Week 7:

* Learn the complete IAM system: user, group, role, policy, permission boundary.
* Understand AWS authentication & authorization mechanisms, how to write JSON policies, and how policy evaluation works.
* Get familiar with AWS Organizations, Organizational Units (OU), and Service Control Policies (SCP).
* Practice IAM + Organization labs to understand how large-scale account management works.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                               | Start Date | Completion Date | Reference                                                                                                      |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | -------------------------------------------------------------------------------------------------------------- |
| 2   | - Learn the IAM fundamentals: user, group, role, policy <br> - Understand policy evaluation: explicit deny, implicit deny, allow                                                                                   | 06/07/2026 | 06/07/2026      | <https://youtu.be/tsobAlSg19g?si=9f3mlIWPtrCcNuKg> <br><br> <https://youtu.be/N_vlJGAqZxo?si=e8oiWCObco95CoKh> |
| 3   | - **Hands-on:** <br>&emsp; + Create user/group/role <br>&emsp; + Attach inline & managed policies <br>&emsp; + Test S3/EC2 access under different policies <br> - Learn permission boundaries and session policies | 07/07/2026 | 07/07/2026      | <https://000028.awsstudygroup.com>                                                                             |
| 4   | - Study AWS Organizations: OU structure, multi-account setup <br> - Understand SCP concepts, deny list vs allow list                                                                                               | 08/07/2026 | 08/07/2026      | <https://youtu.be/5oQY8Rogz9Y?si=h8DlUb8ZLI4HbbvM> <br><br> <https://youtu.be/NW1xrMkNMjU?si=dhT0T3y2JYVK8QwT> |
| 5   | - **Hands-on:** <br>&emsp; + Create Organization + OU <br>&emsp; + Apply SCP deny EC2 / deny S3 <br>&emsp; + Verify SCP effectiveness combined with IAM policies <br>&emsp; + Rearrange OU, remove SCP             | 09/07/2026 | 09/07/2026      | <https://000030.awsstudygroup.com> <br><br> <https://000044.awsstudygroup.com/>                                |
| 6   | - **Team activity:** <br>&emsp; + Discuss workshop ideas <br>&emsp; + Plan execution <br>&emsp; + Divide tasks for workshop                                                                                        | 10/07/2026 | 10/07/2026      |                                                                                                                |

### Week 7 Achievements:

* **Summary:**  
  * This week I learned the foundation of AWS access management, including IAM and Organizations. I now understand how policy evaluation works, how to write JSON policies, and how SCPs apply across multi-account environments.

* **Theory learned:**  
  * Concepts of User – Group – Role – Policy and how evaluation works  
  * Inline policy, managed policy, permission boundary  
  * AWS Organizations structure, OU  
  * SCP concepts and differences compared to IAM policies  
  * Landing Zone & Control Tower overview  

* **Hands-on labs:**  
  * Create user/group/role and test different access levels  
  * Write JSON policies and test allow/deny behaviors  
  * Create Organization, OU, and apply SCP  
  * Validate SCP + IAM policy combinations in practice
