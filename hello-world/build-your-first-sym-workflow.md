---
description: Deploy an Access Workflow in only a few minutes
---

# Build Your First Sym Workflow

You're done waiting for that access request to go through — customers are waiting for a fix and you're waiting for an access request JIRA ticket to be answered. Or, you noticed that everyone on your team has access to sensitive customer data _all the time_. You're worried about an attack, so you decide to fix the problem for good.

{% hint style="info" %}
Goal: configure a Workflow for granting temporary access to staging and production S3 buckets. Requests will be initiated and then peer-approved in Slack. The requester will automatically be granted the appropriate IAM role.
{% endhint %}

![](../.gitbook/assets/image%20%282%29.png)

  
The Sym [Access Template](../sym-concepts/templates/access-template.md) is perfect for this job and can get you set up with a few lines of [Terraform](https://www.terraform.io/) and Python. Terraform is a configuration language that allows you to provision Sym Workflows and other infrastructure. Your organization doesn't have to already use Terraform to start using Sym!

## Provision the Workflow and Tie In Sensitive Resources

The first of two required files is `infra_access.tf`, a Terraform file where we'll provision the Sym Access Workflow, declare which resources can be accessed through this Workflow, and connect the Python file for customization.

{% code title="infra\_access.tf" %}
```javascript
//(1) Brings in Sym's custom Terraform provider
provider "sym" {
  org = "healthy-health" //Your org name goes here
}

//Creates the main Access Workflow resource
resource "sym_flow" "infra_access" {
  handler = {
    template = "sym:access:1.0"    
    impl = file("infra_access.py")  //Customize the template (later!)
  }
  
  slack_source {
    shortcut = "access-request"  //Allows easy triggering of this workflow from Slack
  }

  params = {
    strategies = sym_strategies.escalation_strategies //Ties to line 37
  }
}


//(2) Declares the resources accessible by peer-approval
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


//(3) Connects the Workflow (line 1) and sensitive resources (line 18)
resource "sym_strategies" "escalation_strategies" {
  sym_resources_strategy {
    label = "Sensitive Buckets"
    //allowed_values references the resources you want to protect
    allowed_values = [sym_resources.staging_bucket, sym_resources.production_bucket]
  }
}
```
{% endcode %}

In the first section, we bring in Sym's [Terraform provider](https://www.terraform.io/docs/providers/index.html) and declare the main Sym Workflow resource. This is where you put in your organization's name and set your own Slack shortcut for triggering the Workflow.

The second section is where you declare which resources you'd like to protect. The third section ties the first two together!

## Who Can Approve Requests?

The Access Template has a single required Python function: the `get_approver` [reducer](../sym-concepts/python-sdk/reducers.md). This function takes in a request and returns the set of people who can approve it.

{% code title="infra\_access.py" %}
```python
from sym.annotations import reducer
from sym import events, slack

# Takes a request and returns the set of approvers
@reducer
def get_approver(request):
    # Allows anybody in the #my-team-access-requests Slack channel
    # to approve access requests
    return slack.channel("#my-team-access-requests") 
```
{% endcode %}

This is the final step of wiring up your Workflow: determining who can approver requests to those staging and production S3 buckets from `infra_access.tf`. In this case, anyone on our team can hit approve and give us temporary access. 

## Push it Live!

Ready to fix that data access problem for good? Run `terraform apply` with `infra_access.tf` and `infra_access.py` in the same directory. Your Workflow is done!

Now, check out what it looks like to _use_ this Workflow.

{% page-ref page="using-the-workflow.md" %}



