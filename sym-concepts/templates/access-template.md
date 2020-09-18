---
description: >-
  The Access Template allows you to grant temporary access to sensitive
  resources like EC2 instances, S3 buckets, or databases.
---

# Access Template

The Access Template allows users to request _temporary_ and _auto-expiring_ access to sensitive resources. There are 5 steps for each access request: `prompt`, `request`, `approval`, `escalate`, `denial`. 

Each of these steps is an opportunity to customize behavior through[ SDK actions](../python-sdk/).

The Access Template's only required function to implement is the `get_approver()` [reducer](../python-sdk/reducers.md).



