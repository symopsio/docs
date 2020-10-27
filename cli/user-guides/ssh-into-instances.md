---
description: The Sym CLI can be used for SSH after you've been granted access.
---

# SSH Into Instances

The `sym` CLI has native support for using your escalated privileges to SSH directly into an instance, without any additional authentication. You can use the hostname or IP address of an instance, or just use the Instance ID \(e.g. `i-123456789ascd`\).

### Steps

You need to SSH into a `prod` EC2 instance with ID `i-123456789ascd` . Your organization wants to carefully manage that access so there is an audit trail, and access requests can be efficiently approved or denied. So you'll use Sym to request access and then SSH into the instance:

1. Make sure you've [installed](../setup/install/) Sym and [logged in](../setup/login.md). For this guide, we'll assume you now see a Sym resource called `prod` when you [list resources](../setup/list-resources.md).
2. [Request access](../setup/request-access.md) to the `prod` resource and wait for it to be approved. 
3. Use the `sym ssh` command:

```bash
$ sym ssh prod i-123456789ascd
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-1111-aws x86_64)

Last login: Mon Aug  3 06:44:30 2020 from 127.0.0.1
ubuntu@ip-172-31-78-22:~$ hostname
ip-172-31-78-22
ubuntu@ip-172-31-78-22:~$
```

{% hint style="success" %}
Remember,  you can also use the `SYM_RESOURCE` environment variable to make the `ssh` command's interface more familiar:

```text
$ SYM_RESOURCE=prod sym ssh 34.231.78.150
```
{% endhint %}

{% hint style="warning" %}
The user for the SSH session is configured by your organization, and currently cannot be changed when running `sym ssh`. 
{% endhint %}



