---
description: Initial install of Sym CLI
---

# Install

Installing the `sym` CLI is as simple as running one command:

```bash
curl https://install.symops.io/sym.sh | bash
```

{% hint style="info" %}
If you prefer to install each component individually use the instructions below!
{% endhint %}

{% tabs %}
{% tab title="pipx" %}
You can use [`pipx`](https://pipxproject.github.io/pipx/#install-pipx) to manually install the CLI \(you will need to first install `pipx` if you haven't already\).

```bash
pipx install sym-cli
```
{% endtab %}

{% tab title="pip" %}
You can use `pip` to install the CLI globally.

```bash
pip install --user sym-cli
```
{% endtab %}
{% endtabs %}

If your install succeeded, you should now have `sym` in your path.

```bash
$ sym version
0.0.54
```

### AWS CLI and Session Manager Plugin

Using SSH with the CLI requires that you have both the AWS CLI and the AWS Session Manager CLI Plugin installed and on your path. Sym uses Session Manager to securely connect to EC2 instances in your environment.

* [AWS CLI Installation Instructions](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
* [AWS Session Manager Installation Instructions](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html)

{% hint style="info" %}
To install the Session Manager Plugin on ArchLinux, you can use `rpm2archive` to pull out the session-manager-plugin binary and add it to your path. 
{% endhint %}



