---
description: >-
  Use this so you don't have to specify the same sym resource on every sym
  command
---

# Setting a Default Resource

Prepending `SYM_RESOURCE` to every command can get tiresome. You can specify a default resource to run commands against using `sym defaults`, so that you only have to specify one manually when you want to override that default.

```bash
$ sym defaults:set resource staging
$ sym defaults resource
staging
$ sym resources
prod (Prod)
* staging (Staging)
dogfood (Dogfood)
$ sym ssh i-05d82887a11ef35d2
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-1111-aws x86_64)
```

{% hint style="info" %}
You can unset a default with `sym defaults:unset resource`.
{% endhint %}

