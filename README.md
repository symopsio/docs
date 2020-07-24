---
description: >-
  The Sym SDK empowers engineering teams to easily roll out unobtrusive security
  workflows.
---

# Sym SDK

The Sym SDK is currently in private beta. For more details, please reach out, or read on for a demo. This demo will initially focus on using Sym to protect sensitive infrastructure resources.

### Benefits

To increase the security and audibility of infrastructure access, companies should strive to put a peer-approval process around sensitive resources, with a low default access level, and short grants of escalated privileges upon approval. This happens to satisfy many compliance requirements, namely:

* **HIPAA Privacy Rule**: Minimum Necessary Requirement
* **HIPAA Security Rule**: Access Authorization, Unique User Identification, Automatic Logoff
* **SOC 2**: Common Controls 4 \(Monitoring Activities\) and 6 \(Logical and Physical Address\)

### Workflow

Without Sym, one might decide to implement an approval and access workflow using JIRA tickets, and a single task queue that is owned by the SecOps team. This would require SecOps to manually review requests, escalate engineers, and revoke privileges at a later date. Such a workflow is prone to long turnaround times for engineers, wasted time for ops, and access drift as a result of forgotten grants.

With Sym, we can easily implement a compliant policy that distributes approvals and gets engineers short-lived access in seconds, while keeping Ops out of the critical path.

When you're done with this demo, you'll have built a smooth approval flow that you can initiate from your command line, with approvals granted via a message in a Slack channel, and fully-automated privilege escalations.



