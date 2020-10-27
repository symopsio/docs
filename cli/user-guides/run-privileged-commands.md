---
description: 'Use exec to run any privileged commands, like you can with other tools.'
---

# Run Privileged Commands

Once approved for a given resource, you can execute commands with temporary elevated credentials for that resource using `sym exec RESOURCE -- command`.

### Steps

1. Make sure you've [installed](../setup/install/) Sym and [logged in](../setup/login.md). For this guide, we'll assume you see a Sym resource called `prod` when you [list resources](../setup/list-resources.md).
2. [Request access](../setup/request-access.md) to the `prod` resource and wait for it to be approved.
3. Use the `sym exec` command:

For example, after successfully requesting access to the `prod` resource, you can run `aws-cli`commands against your production environment.

```bash
$ sym exec prod -- aws ec2 describe-instances
{
    "Reservations": [
        {
            "Groups": [],
            "Instances": [
                { ... }
            ],
            ...
        }
    ]
}
```

{% hint style="info" %}
`sym exec`works very similarly to `aws-okta exec` or `aws-vault exec`
{% endhint %}

If you need a copy of the credentials for the elevated role, you can simply access the environment variables.

```bash
$ sym exec prod -- env | grep AWS
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=%%%
AWS_SECRET_ACCESS_KEY=%%%
AWS_SESSION_TOKEN=%%%
AWS_SECURITY_TOKEN=%%%
AWS_SESSION_EXPIRATION=2020-04-16T11:16:27Z
```

{% hint style="info" %}
You can also use the `SYM_RESOURCE` environment variable to specify the resource, which makes commands like `exec` more ergonomic:

```text
$ SYM_RESOURCE=prod sym exec env
```
{% endhint %}

