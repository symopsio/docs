---
description: This is an example of an ephemeral infrastructure access workflow.
---

# Infra Access Demo

Welcome to the [Sym](https://symops.com/) SDK Demo. We'll be exploring an infrastructure access use case, but the Sym platform supports many more access workflows.

This is an example of the ephemeral infrastructure access workflow implemented using our Terraform provider and Python SDK.

## Video

You can see a demo of the below flow end-to-end below. We recommend checking it out, along with our [product page](https://symops.com/product), before continuing.

{% embed url="https://www.youtube.com/watch?v=1r6pQDHRJo8&feature=youtu.be" %}



## Overview

> To increase the security and audibility of infrastructure access at Healthy Health, we've decided to put a peer-approval process around sensitive resources, with a low default access level, and short grants of escalated privileges upon approval.
>
> When you're done with this tutorial, you'll have a smooth approval flow that you can initiate from Slack or your command line, with approvals granted via a message in a Slack channel, and fully-automated privilege escalations.

There are a few extra features implemented, such as:

* Auto-approval to the resource if the engineer is on-call on PagerDuty
* Auto-denial to the resource if the engineer does not satisfy a policy, laid out as a Rego script \(the language of [OPA](https://www.openpolicyagent.org/docs/latest/policy-language/)\)
* Different approvers based on the type of the resource being requested
* Integrations with Slack and Okta for approval groups

