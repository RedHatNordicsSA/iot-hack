Cockpit
=======

Installs and enables Cockpit management tool into RHEL. Role also adds proper SSL certificate using Let's Encrypt service.

Requirements
------------

Route53 account with domain configured. Also Let's Encrypt account must exist.

Role Variables
--------------

You need to define your FQDN used for the SSL certificate, as well as your email for let's encrypt notifications.

* letsencrypt_email: 
* cockpit_fqdn:

Dependencies
------------

Tool assumes you have installed let's encrypt utility, and aws credentials are in place. 

Also, you need to have a policy in AWS IAM for the AWS user to verify your domain:

```
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "route53:GetChange",
                "route53:ListHostedZones",
                "route53:ListResourceRecordSets"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "route53:ChangeResourceRecordSets",
            "Resource": "arn:aws:route53:::hostedzone/your_zone_id_here"
        }
    ]
}
```


Example Playbook
----------------

```
- name: install cockpit 
  hosts: myhost
  tasks:
  - include_role:
      name: common-packages
  - include_role:
      name: aws_config
  - include_role:
      name: cockpit
```

License
-------

GPL

Author Information
------------------

itengval@redhat.com
