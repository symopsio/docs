---
description: >-
  The Sym CLI can tunnel an SSH connection to any instance, allowing any
  application which works over SSH to use your escalated privileges.
---

# SSH Tunnel

While most applications can use temporary credentials generated from your escalated privileges via `sym exec`, occasionally you'll want to simply open an SSH connection to an instance and let an application use it. Sym has native support for several applications, including [Ansible](ansible.md), but also exposes a `ProxyCommand` for general SSH usage.

To use an application that connects over SSH with Sym, simply ensure the following arguments are passed to the `ssh` binary on connection \(how to do this varies from application to application\):

```bash
-o ProxyCommand="sh -c 'sym ssh-session-with-key RESOURCE --host %h --port %p'" -o StrictHostKeyChecking=no
```

{% hint style="info" %}
You'll need to replace `RESOURCE` with the Sym resource where the instance is located. Alternatively, you can ensure that the `SYM_RESOURCE` environment variable is set and passed through to `ssh`.
{% endhint %}

{% hint style="success" %}
You can then specify the instance to your application by IP address, hostname, or Instance ID. 
{% endhint %}

