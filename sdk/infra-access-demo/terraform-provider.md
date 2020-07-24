# Terraform Provider

The first step is to provision a new Sym Flow with our custom Terraform provider. 

Flows require a `template`, and an `impl`, which is a Python file containing code written with the Sym SDK \(we'll explore this later\).

{% hint style="success" %}
For this demo, we'll use the `sym:access` template, which is provided by Sym.
{% endhint %}

```text
provider "sym" {
  org = "healthy-health"
}

resource "sym_flow" "infra_access_flow" {
  handler = {
    template = "sym:access:1.0"
    impl = file("infra_access.py")
  }
```

## Sources

Flows can have various sources which produce triggering Events. For our demo, we'll add a Slack shortcut.

```text
  slack_source {
    shortcut = "foobar" # Allows easy triggering of this workflow from Slack
  }
```

## Parameters

A `sym_flow` also takes a set of parameters. In this case, we'll declare two, then define them below.

| Name | Description |
| :--- | :--- |
| `strategies` | A set of Sym Strategies, which define the resources we're protecting access to, and let Sym know how to automatically grant and revoke access. |
| `custom_fields` | A Sym Form instance, which declares the fields to be collected from a user who's requesting access to a resource. |

```text
  params = {
    strategies = sym_strategies.escalation_strategies
    custom_fields = sym_form.request_fields
  }
}  
```

## Forms

Sym Forms let you specify a group of fields to collect user input. Let's collect two fields, a `reason` for the access request, and optionally, the user's favorite color.

```text
resource "sym_form" "request_fields" {
  field "reason" {
    type = "string"
    required = true
  }


  field "fave_color" {
    type = "string"
    label = "Favorite Color"
    required = false
  }
}
```

## Strategies

Sym Strategies define how Sym automatically escalates and de-escalates users after a successful access request. 

The `sym_resources` primitive provides a handy abstraction around common AWS resources. For this demo, we'll protect two S3 buckets.

```text
resource "sym_resources" "staging_bucket" {
  type = "s3"
  label = "Staging"
  arn = "aws::s3::::foobar-staging"
}

resource "sym_resources" "production_bucket" {
  type = "s3"
  label = "Prod"
  arn = "aws::s3::::foobar"
}
```

The `sym_resources_strategy` takes a set of `sym_resources`, and creates an IAM Role and Group for each. It instructs Sym to escalate users by temporarily placing them in the Group \(which can assume the Role\).

```text
resource "sym_strategies" "escalation_strategies" {
  sym_resources_strategy {
    label = "Sensitive Buckets"
    allowed_values = [sym_resources.staging_bucket, sym_resources.production_bucket]
  }
}
```

