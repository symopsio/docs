---
description: Common use cases for Sym CLI after you've installed and logged in
---

# User Guides

In general, the Sym CLI is used by engineers who want to use temporary, elevated permissions to sensitive resources - SSH into production instances, running ansible playbooks over a fleet of instances, and more.  

Sym users [request access](../setup/request-access.md) to the a [resource](../setup/list-resources.md) - for example, `prod`. Once that access request is granted, behind the scenes the user is granted temporary escalated permissions. They can use those permissions by issuing commands with the Sym CLI. 

{% page-ref page="ssh-into-instances.md" %}

{% page-ref page="ansible.md" %}

{% page-ref page="run-privileged-commands.md" %}

{% page-ref page="ssh-tunnel.md" %}





