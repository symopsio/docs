---
description: >-
  Each Workflow is customized through the Python SDK. Some functions are
  required for certain Templates.
---

# Python SDK

Sym's SDK allows you to inject your own logic at key steps in Workflows. There are three mechanisms for customizing the logic of a Workflow: `@hook`, `@reducer`, and `@action` .

[Hooks](hooks.md) occur _before_ key steps of the workflow, and offer the opportunity to bypass, short-circuit, or alter the flow of steps, by emitting `events`.

[Reducers](reducers.md) take the current state of the workflow as input, and return a single value \(whose type depends on the specific reducer\) to a given step.

[Actions](actions.md) are opportunities to execute additional side effects _after_ a step.

