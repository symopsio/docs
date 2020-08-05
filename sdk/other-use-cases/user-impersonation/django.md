---
description: Protect a sensitive endpoint in Django with Sym.
---

# Django

The Sym Access template can easily be used to put a Slack-based peer approval workflow in front of an arbitrary view in Django. 

## Integration

### In Django

The `sym.django` module provides a simple request decorator which, upon visiting a route mapping to the view, requests access on behalf of the current user while displaying a waiting screen.

```python
import json
from django.http import HttpResponse
from sym.django import sym_init, require_approval

sym_init(token=ENV['SYM_TOKEN'])

@require_approval(flow="impersonate")
def impersonation_demo(request):
  success = "<b style='color: red;'>Successfully Impersonating</b>"
  user = f"<code>{json.dumps(request.user)}</code>"
  html = f"<html><body>{success}: {user}</body></html>"
  return HttpResponse(html)
```

### In Sym

The `sym:access` template provides a quick way to provision a Sym workflow which receives the access request from the browser, routes it to a Slack channel, and returns the approval to the browser \(via the Sym API\) once granted.

To get going, all you have to do is use our Terraform provider to provision a `flow` with the template, specifying a `user_id` input field. You also need to supply a simple Python file that configures the channel to route requests to.

{% tabs %}
{% tab title="main.tf" %}
```text
provider "sym" {
  org = "klaviyo"
}

resource "sym_flow" "impersonation_flow" {
  handler = {
    template = "sym:access:1.0"
    impl = file("impersonation.py")
  }

  params = {
    custom_fields = sym_form.request_fields
  }
}  

resource "sym_form" "request_fields" {
  field "user_id" {
    type = "string"
    required = true
  }
}
```
{% endtab %}

{% tab title="impersonation.py" %}
```python
from sym.annotations import reducer
from sym.integrations import slack

@reducer
def get_approver(event):
    return slack.channel("#impersonations")
```
{% endtab %}
{% endtabs %}

## Demo

You can see this gem and workflow demoed [here](https://impersonation-demo.symops.io). The requests will go to the `#impersonations` channel of the [Sym Demo](https://symdemo.slack.com) Slack workspace, where they can be approved by anyone in the channel in realtime.

