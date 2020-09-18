---
description: Actions are opportunities to execute additional side effects after a step
---

# Actions

An `action` is associated with a specific step in a Template. They're named as `after_*`, like `after_prompt` or`after_approval` for the Access Template.

{% code title="infra\_access.py" %}
```python
from sym.annotations import action

# @action implements a side-effect
# Available actions: after_prompt, after_request, after_approval

@action # Actions are like callbacks after a step has occured
def after_prompt(event):
    """
    Executed after the requesting user is prompted for details about their request.
    """

    pass

...
```
{% endcode %}

