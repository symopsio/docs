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
You can also specify a `SYM_ANSIBLE_RESOURCE` to have the local Ansible command assume a different Sym Resource Role than the one used for the SSH connection.
{% endhint %}

{% hint style="warning" %}
If you include an`AWS_PROFILE` , Ansible will assume that IAM Role instead of the Sym Resource Role when executing locally.
{% endhint %}

You can also run `ansible` commands in a similar way:

```bash
$ SYM_RESOURCE=staging sym ansible all -m ping -vvv

34.230.78.151 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "invocation": {
        "module_args": {
            "data": "pong"
        }
    },
    "ping": "pong"
}
```

{% hint style="success" %}
You can use `sym defaults:set resource staging` to avoid having to include `SYM_RESOURCE=staging` in every command!
{% endhint %}

