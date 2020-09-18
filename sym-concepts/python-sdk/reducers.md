---
description: Reducers translate the current event state to a single value
---

# Reducers

A `reducer` injects key logic into Workflows by taking the current state of the Workflow as input, and returning a single value. For example, an access request must be routed to the right person or group for approval. The `get_approver` reducer takes an event, containing the requester's identity, what they want to access, for how long, and more, and returns the list of valid approvers.

{% code title="infra\_access.py" %}
```python
from sym.annotations import reducer
​
# @reducer _must_ return a single value (type depends on the reducer)
# Available reducers: get_approver
​
@reducer # Reducers are mandatory, and provide dynamic config values
def get_approver(event):
    return slack.channel("#ops")

​
...
```
{% endcode %}

