---
description: Check which resources you have available after logging in
---

# List Resources

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

