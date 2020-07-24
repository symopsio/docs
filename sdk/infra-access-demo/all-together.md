---
description: >-
  A complete peer-approved chatops-based ephemeral infra access workflow in
  three files.
---

# All Together

{% tabs %}
{% tab title="infra\_access.py" %}
```python
from sym.annotations import hook, action, reducer
from sym import okta, slack, pagerduty, rego, events, schedule

@reducer
def get_approver(request):
    if request.resource.type == "s3":
        return okta.group("data-security")
    return slack.channel("#ops")

from sym import pagerduty, events

@hook
def on_request(event):
    if pagerduty.on_call(event.user):
        yield schedule("tomorrow", "sym:incident_follow_up", event)
        return events.approval.approved()

    if not rego.eval("./approval.rego", {"user": event.user}):
        return events.approval.denied()
```
{% endtab %}

{% tab title="infra\_access.tf" %}
```text
provider "sym" {
  org = "healthy-health"
}

resource "sym_flow" "infra_access_flow" {
  handler = {
    template = "sym:access:1.0"
    impl = file("infra_access.py")
  }
  
  slack_source {
    shortcut = "foobar" # Allows easy triggering of this workflow from Slack
  }

  params = {
    strategies = sym_strategies.escalation_strategies
    custom_fields = sym_form.request_fields
  }
}

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

resource "sym_strategies" "escalation_strategies" {
  sym_resources_strategy {
    label = "Sensitive Buckets"
    allowed_values = [sym_resources.staging_bucket, sym_resources.production_bucket]
  }
}
```
{% endtab %}

{% tab title="requesters.rego" %}
```
default allow = false

allow {
  user_is_fulltime
}

user_is_fulltime {
  not user_is_contractor
}

user_is_contractor {
  some i
  data.user.groups[i] == "contractors"
}
```
{% endtab %}
{% endtabs %}

