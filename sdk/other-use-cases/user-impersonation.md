# User Impersonation

A common use case of the Sym Access template is to wrap a Slack-based peer-approval flow around application-level user impersonation, or "god mode". For an overview of the Sym SDK, please first read through the SDK Demo.

{% page-ref page="../infra-access-demo/" %}

## Lambda Strategy

With the lambda strategy, customers can deploy a Lambda function to their secure cloud environment with any custom escalation logic. This often involves making a change to a record in a database, or an internal API call, to enable the requested user impersonation. 

Lambda Strategies are declared with the `lambda_strategy` resource in your Terraform config, which will automatically provision and deploy a Lambda.

```text
resource "sym_form" "request_fields" {
  field "user_id" {
    type = "string"
    required = true
  }
}

resource "sym_strategies" "escalation_strategies" {
  lambda_strategy {
    id = "my-lambda"
    label = "User Impersonation"
    code = file("my_lambda_code.js")
  }
}
```

If your Lambda already exists, you can simply specify the `arn` instead of the `code`.

## API Trigger

While requests will still be routed to Slack for approval, the user impersonation workflow is often _triggered_ with a Google Docs-style permission window in the web UI of the customer's application. When a user tries to access the endpoint which allows them to impersonate a user, their request is held while approval is fetched.

This flow can be implemented by the user using Sym's API, or you can use one of our handy web framework integrations! For example, we have a Ruby gem, `sym-impersonate`, which will wrap any endpoint in a Sym Approval workflow with only two lines of code.

When a user tries to visit the action, they'll be met with a Sym-rendered screen which walks them through getting approval, without you having to change your endpoint logic at all.

{% tabs %}
{% tab title="Rails" %}
```ruby
class ImpersonationController < ApplicationController
 include Sym::Impersonate::Concern

 impersonate_with :impersonate

 def impersonate
   # app-specific impersonation logic here
 end
end
```
{% endtab %}

{% tab title="Django" %}
```python
from sym.django import require_approval

@require_approval(flow="impersonate")
def impersonation_demo(request):
  # app-specific impersonation logic here
  pass
```
{% endtab %}
{% endtabs %}

## Demo

You can see this gem and workflow demoed [here](https://impersonation-demo.symops.io). The requests will go to the `#impersonations` channel of the [Sym Demo](https://symdemo.slack.com) Slack workspace, where they can be approved by anyone in the channel in realtime.

