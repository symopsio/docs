# SSH Into Instances

The `sym` CLI also has native support for using your escalated privileges to SSH directly into an instance, without any additional authentication. You can use the hostname or IP address of an instance, or just use the Instance ID \(e.g. `i-123456789ascd`\).

```bash
$ sym ssh prod i-123456789ascd
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-1111-aws x86_64)

Last login: Mon Aug  3 06:44:30 2020 from 127.0.0.1
ubuntu@ip-172-31-78-22:~$ hostname
ip-172-31-78-22
ubuntu@ip-172-31-78-22:~$
```

{% hint style="success" %}
Remember,  you can also use the `SYM_RESOURCE` environment variable to make the `ssh` command's interface more familiar:

```text
$ SYM_RESOURCE=prod sym ssh 34.231.78.150
```
{% endhint %}

{% hint style="warning" %}
The user for the SSH session is configured by your organization, and currently cannot be changed when running `sym ssh`. 
{% endhint %}



