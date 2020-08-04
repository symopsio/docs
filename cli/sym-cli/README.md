---
description: >-
  The Sym CLI allows you to use your escalated privileges for various
  operations.
---

# Sym CLI

## Installing

Installing the `sym` CLI is as simple as running one command:

```bash
$ curl https://install.symops.io/sym.sh | bash
```

{% hint style="info" %}
If the install script fails for any reason, we have a few alternatives.
{% endhint %}

{% tabs %}
{% tab title="pipx" %}
You can use [`pipx`](https://pipxproject.github.io/pipx/#install-pipx) to manually install the CLI \(you will need to first install `pipx` if you haven't already\).

```bash
$ pipx install sym-cli
```
{% endtab %}

{% tab title="pip" %}
You can use `pip` to install the CLI globally.

```bash
$ pip install --user sym-cli
```
{% endtab %}

{% tab title="brew" %}
You can use `brew` to install the CLI globally on macOS or Linux.

```bash
$  brew install symopsio/tap/sym
```
{% endtab %}
{% endtabs %}

If your install succeeded, you should now have `sym` in your path.

```bash
$ sym version
0.0.13
```

## Setup

Before you can use `sym`, you'll need to let us know who you are! 

```bash
$ sym login
Org: your-org-slug-here
Email: you@org.com
```

{% hint style="info" %}
If you don't know your organization's slug, please contact support.
{% endhint %}

## Listing Resources

The`sym` CLI can give you a list of the resources you can request access to. Often, these resources map to the different environments where you might want to run commands.

```bash
$ sym resources
prod (Prod)
staging (Staging)
dogfood (Dogfood)
```

{% hint style="info" %}
The Sym CLI can be used _after_ you've been approved for a particular resource. To request access, use the `/sym` command in Slack.
{% endhint %}

## Run Privileged Commands

Once approved for a given resource, you can execute commands with temporary elevated credentials for that resource.

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

