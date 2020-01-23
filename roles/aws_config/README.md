aws_config
==========

This role sets up AWS config file for awx command line tool, along with the given credentials.

Requirements
------------

At the moment the awx tool is not installed by the role.


Role Variables
--------------

Add these variables into vault, they'll be stored to config file

* aws_access_key_id: add_your_key_id_into_vault
* aws_secret_access_key: add_your_secret_to_vault


Dependencies
------------

none

Example Playbook
----------------

```
- name: write awx config file
  hosts: all
  tasks:
  - include_role:
      name: aws_config
```

License
-------

GPL

Author Information
------------------

itengval@redhat.com