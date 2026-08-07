---
title : "Các bước chuẩn bị"
date : 2026-08-02
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### IAM permissions
Gắn IAM permission policy sau vào tài khoản AWS user của bạn để triển khai và dọn dẹp tài nguyên trong workshop này.
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DeployCoreInfrastructure",
            "Effect": "Allow",
            "Action": [
                "cloudformation:*",
                "ec2:*",
                "s3:*",
                "cloudwatch:*",
                "logs:*",
                "sns:*"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ManageEC2InstanceProfileRole",
            "Effect": "Allow",
            "Action": [
                "iam:CreateRole",
                "iam:DeleteRole",
                "iam:PutRolePolicy",
                "iam:DeleteRolePolicy",
                "iam:AttachRolePolicy",
                "iam:DetachRolePolicy",
                "iam:GetRole",
                "iam:ListRoles",
                "iam:CreateInstanceProfile",
                "iam:DeleteInstanceProfile",
                "iam:AddRoleToInstanceProfile",
                "iam:RemoveRoleFromInstanceProfile",
                "iam:GetInstanceProfile"
            ],
            "Resource": "*"
        },
        {
            "Sid": "RestrictPassRoleToEC2Only",
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "ec2.amazonaws.com"
                }
            }
        }
    ]
}
```

#### Khởi tạo tài nguyên bằng CloudFormation

![create stack](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.2-Prerequisite/1.png)

* Nhấn **Create with new source**.

![create stack](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.2-Prerequisite/2.png)

* Chọn và upload file YAML để tạo stack.

![create stack](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.2-Prerequisite/3.png)

![create stack](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.2-Prerequisite/4.png)

* Nhập tên stack và email để nhận thông báo alarm từ Amazon SNS.

![create stack](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.2-Prerequisite/5.png)

![create stack](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.2-Prerequisite/6.png)

* Thêm tag cho stack.

![create stack](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.2-Prerequisite/7.png)

![create stack](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.2-Prerequisite/8.png)

* Nhấn **Submit** và chờ quá trình triển khai CloudFormation hoàn thành.

![finish](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.2-Prerequisite/10.png)

* Các tài nguyên đã được tạo thành công.

![finish](/fcaj-workshop-quangttse196220-fptu/images/5-Workshop/5.2-Prerequisite/11.png)
