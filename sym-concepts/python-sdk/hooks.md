---
description: Hooks occur before key steps of the Workflow
---

# Hooks

A `hook` is associated with a specific step in a Template. They're named as `on_*`, like `on_prompt`, `on_request`, or `on_approval` for the Access Template. 

{% code title="infra\_access.py" %}
```python
from sym.annotations import hook
​
# @hook _can_ return a sym.event
# Available hooks: on_prompt, on_request, on_approval
​
@hook # Hooks are optional, and can change control flow by returning events
def on_prompt(event):
    """
    Executed before the requesting user is prompted for details about their request.
    """
​
    pass
​
...
```
{% endcode %}

