# Gated Deploys

A common application of the Sym Access workflow is to use as a human approval step in a CI/CD process. For an overview of the Sym SDK, please first read through the SDK Demo.

{% page-ref page="../infra-access-demo/" %}

## Human Approvals

Services like Github Actions and CircleCI have primitives for [human approvals](https://circleci.com/docs/2.0/workflows/#holding-a-workflow-for-a-manual-approval), but these primitives are often not compliance-friendly: it is very cumbersome to use them to get the level of auditability and reporting necessary to demonstrate compliance, and it is impossible to limit the set of approvers without replicating the functionality from scratch.

Sym offers drop-in replacements \(specifically an Action and an Orb\) for these human approval steps, which trigger a Sym workflow similar to the ones configured above. You get all the benefits of dynamic routing, evidence collection, and customizability that you've come to enjoy, with a native integration to your CI/CD provider. The only difference is how the workflow is triggered \(via a CI step, instead of Slack, a CLI, or our API\), and what the escalation strategy is \(in this case, it is a special strategy which allows the workflow to continue\).

## CircleCI Example

In your `.circleci/config.yml`:

```yaml
orbs:
  sym: sym/approvals@1.0.0

jobs:
  test:
    - # test logic
  build:
    - # build logic
   approve:
    - sym/approve
```

In your Terraform config:

```text
resource "sym_strategies" "escalation_strategies" {
  circleci_strategy {
    id = "my-circleci-org"
  }
}
```

