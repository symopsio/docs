---
description: Initial install of Sym CLI
---

# Install

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
{% endtabs %}

If your install succeeded, you should now have `sym` in your path.

```bash
$ sym version
0.0.54
```

