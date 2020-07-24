# Python SDK

The next step is to implement your custom logic for this Flow, using Sym's Python SDK.

## Hooks, Actions, and Reducers

Templates in Sym are composed of a series of pre-defined steps. There are three mechanisms for customizing the logic of a workflow: `@hook`, `@reducer`, and `@action`.

* `hooks` occur _before_ key steps of the workflow, and offer the opportunity to bypass, short-circuit, or alter the flow of steps, by emitting `events`.
* `reducers` take the current state of the workflow as input, and return a single value \(whose type depends on the specific reducer\) to a given step.
* `actions` are opportunities to execute additional side effects _after_ a step,

{% hint style="info" %}
The `sym:access` template defines five steps \(`prompt`, `request`, `approval`, `escalate`, `denial`\), each with an `on_*` hook, and an `after_*` action. The template also defines one `reducer`, `get_approver`, which must be supplied.
{% endhint %}

{% code title="infra\_access.py" %}
```python
from sym.annotations import hook, action, reducer

# @hook _can_ return a sym.event
# @reducer _must_ return a single value (type depends on the reducer)
# @action implements a side-effect

# Available hooks: on_prompt, on_request, on_approval
# Available actions: after_prompt, after_request, after_approval
# Available reducers: get_approver

@reducer # Reducers are mandatory, and provide dynamic config values
def get_approver(event):
    raise NotImplementedError


@hook # Hooks are optional, and can change control flow by returning events
def on_prompt(event):
    """
    Executed before the requesting user is prompted for details about their request.
    """

    pass


@action # Actions are like callbacks after a step has occured
def after_prompt(event):
    """
    Executed after the requesting user is prompted for details about their request.
    """

    pass

...
```
{% endcode %}

## Routing Requests

Let's define the `get_approver` reducer, which provides us the opportunity to map an access request from a user for a specific resource to a set of approvers.

For our demo, we'll say that all requests for access to S3 buckets must be approved by the Data Security team. As a fallback, any other requests will get routed to the `#ops` channel in Slack.

{% hint style="info" %}
The Data Security team is defined as an Okta group, and Sym knows how to figure out which Slack users are in that group.
{% endhint %}

```python
from sym import okta, slack

@reducer # Reducers are mandatory, and provide dynamic config values
def get_approver(request):
    if request.resource.type == "s3":
        return okta.group("data-security")
    return slack.channel("#ops")
```

That's all we need to ship this Flow! However, there might be a few other things we want to implement.

## On-Call Auto Escalation

A common pattern is to allow engineers who are on call to access sensitive resources with a self-approval, to unblock rapid fire-fighting. With Sym's SDK and the Sym Access template, this is as simple as implementing the `on_request` hook.

```python
from sym import pagerduty, events

@hook # Hooks are optional, and can change control flow by returning events
def on_request(event):
    if pagerduty.on_call(event.user):
        return events.access.approved()
```

Another common pattern is to require auto-escalations to be documented in an incident management system when the fire has been quenched. Sym provides a template for incident follow-ups that DMs the user at a later time to ensure policy is followed without getting in the way.

```python
from sym import schedule

...
    if pagerduty.on_call(event.user):
        yield schedule("tomorrow", "sym:incident_follow_up", event)
        return events.access.approved()
```

## Evaluate Policy-As-Code

Many organizations have existing Rego \(OPA\) or Sentinel policy files that may dictate who can request access to a resource. Sym provides integrations for both languages, which can also be used in the `on_request` hook to ensure compliance.

{% code title="requesters.rego" %}
```text
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
{% endcode %}

{% code title="infra\_access.py" %}
```python
from sym import rego

@hook
def on_request(event):
    ...
    if not rego.eval("./requesters.rego", {"user": event.user}):
        return events.approval.denied()
```
{% endcode %}

