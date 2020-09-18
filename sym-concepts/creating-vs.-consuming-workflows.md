---
description: >-
  Sym Workflows are provisioned using our Terraform provider and Python SDK. Sym
  Workflows are kicked off in many ways and connect multiple technologies as
  defined by the Workflow creator!
---

# Creating vs. Consuming Workflows

### Creating Workflows

Workflows are provisioned through Terraform with Sym's custom [Terraform provider](https://www.terraform.io/docs/providers/index.html). Each Workflow follows from a specific Template or Workflow purpose. Instantiating a Workflow requires two files: a Terraform file where the Template is specified, and a Python file where custom logic and integrations are embedded.

Below are stubs of the required files. See the [Hello World](../hello-world/build-your-first-sym-workflow.md) section for a full example.

{% tabs %}
{% tab title="infra\_access.tf" %}
```javascript
provider "sym" {
  org = "healthy-health"
}

resource "sym_flow" "infra_access_flow" {
  handler = {
    template = "sym:access:1.0"
    impl = file("infra_access.py")
  }
  
   slack_source {
    shortcut = "access-request" # Allows easy triggering of this workflow from Slack
  }
  
...
```
{% endtab %}

{% tab title="infra\_access.py" %}
```python
from sym.annotations import hook, action, reducer
from sym import slack

@reducer
def get_approver(request):
    if request.resource.type == "s3":
        return slack.channel("#data-security")
    return slack.channel("#ops")
    
...
```
{% endtab %}
{% endtabs %}

### Consuming Workflows

Once the above `infra_access_flow` has been provisioned with `terraform apply`, engineers can kick off this Workflow with `/access-request` in Slack \(hint: look at `infra_access.tf` line 12\).

This pattern holds for all Workflows. Workflows are provisioned once and then usable by a segment of the organization.



