ddns
====

This role set's up dynamic domain name service entry for a server. If you have a roaming box, e.g. behind 4G or DHCP, this module helps you have constant FQDN name for it.

Requirements
------------

You naturally need to have Route53 account, and domain in there with hostname to use. And you need to have this policy in place:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "route53:GetHostedZone",
                "route53:ListResourceRecordSets",
                "route53:ListHostedZones",
                "route53:ChangeResourceRecordSets",
                "route53:ListResourceRecordSets",
                "route53:GetHostedZoneCount",
                "route53:ListHostedZonesByName"
            ],
            "Resource": "arn:aws:route53:::hostedzone/YOUR_ZONE_ID"
        }
    ]
}
```

Role Variables
--------------

Fill in here the Route53 zone id for your DNS entry. And have DNS entry in cockpit_fqdn variable.

* aws_route53_zoneid:
* cockpit_fqdn:


Dependencies
------------

Role doesn't set AWS credentials or install the AWS tooling. There is another role for it.

Example Playbook
----------------

```
- name: basic initialization playbook for roaming rhel host 
  hosts: all
  tasks:
  - include_role:
      name: common-packages
  - include_role:
      name: aws_config
  - include_role:
      name: ddns
```

License
-------

GPL

Author Information
------------------

itengval@redhat.com