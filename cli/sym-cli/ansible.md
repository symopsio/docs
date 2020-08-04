---
description: The Sym CLI has native support for Ansible.
---

# Ansible

You can use the Sym CLI to run `ansible` or `ansible-playbook` commands with your temporary credentials.

A common invocation of `ansible-playbook` goes something like this:

```bash
$ AWS_PROFILE=staging ansible-playbook nginx.yml
```

Invoking `ansible-playbook` with `sym` looks very similar. You simply have to specify a `SYM_RESOURCE` environment variable, and prefix `ansible-playbook` with `sym`:

```bash
$ SYM_RESOURCE=staging sym ansible-playbook nginx.yml
______________________
< PLAY [Install nginx] >
 ----------------------
 ________________________
< TASK [Gathering Facts] >
 ------------------------
ok: [34.230.78.150]
 ______________________
< TASK [Install nginx] >
 ----------------------
ok: [34.230.78.150]
 ____________
< PLAY RECAP >
 ------------
34.230.78.151: ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

{% hint style="info" %}
Make sure to omit `AWS_PROFILE` if you normally include it.
{% endhint %}

You can also 
