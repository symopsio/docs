---
description: The Sym CLI supports both aws-okta and saml2aws.
---

# Configuring SAML Clients

When Sym is connected to your IDP \(such as Okta\), escalated access to resources is often granted by temporarily adding you to a group that has delegated permission to assume an IAM Role. 

{% hint style="info" %}
This works by using your IdP credentials to obtain a SAML assertion containing the Role you want to assume, which AWS has previously been configured to accept in exchange for temporary [STS](https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html) credentials. 
{% endhint %}

Most organizations handle the IDP-to-STS-via-SAML dance with one of two tools: [`aws-okta`](https://github.com/segmentio/aws-okta) or [`saml2aws`](https://github.com/Versent/saml2aws). Sym supports both of these tools. Normally, `sym` will simply select the best option \(`saml2aws` if you have it installed, else the now-unmaintained `aws-okta`\). 

This is configurable by passing the `--saml-client` flag to `sym`, which defaults to `auto`.

| Value | Behavior |
| :--- | :--- |
| `auto` | Default to the `saml2aws` client, fallback to `aws-okta`. |
| `aws-okta` | Use the `aws-okta` client unconditionally. |
| `saml2aws` | Use the `saml2aws` client unconditionally. |
| `aws-profile` | Use predefined [named AWS profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html) unconditionally. |

A common case for explicitly specifying a value is if both utilities are installed, but only one is set up.

For example, to list all EC2 instances in the `prod` environment using `aws-okta`, even if you have `saml2aws` installed, run the following:

```bash
sym --saml-client=aws-okta exec prod -- aws ec2 describe-instances
```



