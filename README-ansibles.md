# Ansible Tower automation

This doc describes Ansible automation implemented into Tower for the project. There is vault file used in the project for the secrets, and some of the secrets are in Tower instance inventories, also vault encrypted. The following list of templates is available at the Tower:

## Local setup

```
ansible-galaxy install -r roles/requirements.yaml -p roles
ansible-galaxy install -r collections/requirements.yaml -p collections
```

Create inventory file and override vault.yml with your own.

Example inventory file:

```
foo.host.invalid

[all:vars]
ansible_user=foouser
ansible_ssh_private_key_file=~/.ssh/id_rsa
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

```
ansible-playbook -i inventory  run-mqtt-bridge-container.yml --vault-password-file ~/.vault_password
```

Removal:

```
ansible-playbook -i inventory  run-mqtt-bridge-container.yml --vault-password-file ~/.vault_password --extra-var container_state=absent
```


Check out stuff on host:

```
systemctl status mqtt-to-kafka-bridge-container-pod.service
journalctl -u mqtt-to-kafka-bridge-container-pod.service
```

# Setup RHEL Basics

This task is meant to make sure the remote RHEL device is set up properly for the exercise. It runs [setup-rhel-host playbook](https://github.com/RedHatNordicsSA/iot-hack/blob/master/setup-rhel-host.yml'). The following tasks are done:

* Subscribe Linux to Red Hat services
* Install basic set of software we need
* Create AWS config for Route53 services
* Set up Dynamic DNS (DDNS) using Route53. This is to have public hostname for the box (FQDN) no matter where it roams.
* Set up Cockpit web gui for server management. Also set cockpit with proper Let's Encrypt SSL certificates.
* Register host to [Red Hat Insights](https://www.redhat.com/en/technologies/management/insights) service

# Create OCP empty project

This task sets up a new empty project into OpenShift.

# Create OCP project

This task sets up a new project into OpenShift with:

* Admin user for the project
* Set's default NetworkPolicy with firewall preventing accessing the project services for anyone else but OpenShift system internal services.

# Install 3Scale APIcast Gateway on RHEL

Installs 3Scale API GW into RHEL box, along with:

* Login to Red Hat registry
* Systemd control file, service enabled across reboots
* Firewall opened
* APIGW secret from vault

# Delete 3Scale APIcast Gateway from RHEL

Removes 3Scale APIgw container from RHEL.

# Install MQTT broker on RHEL

Installs Mosquitto broker into RHEL box, along with:

* Systemd control file, service enabled across reboots

# Delete MQTT broker from RHEL

Removes Mosquitto broker container from RHEL. Data is left behind.

# Install Node-Red on RHEL

Installs Node-Red into RHEL box, along with:

* Systemd control file, service enabled across reboots
* Firewall opened
* Node-Red flows stored into given directory outside of container.

# Delete Node-Red from RHEL

Removes Node-Red container from RHEL. Data is left behind.

# Nuke OCP project

This tasks deletes the given project from OpenShift, and anything that was created inside it. Beware, it really nukes it all!

# Ping hosts

This task simply test the ssh connectivity to all hosts in inventory. If it fails, there is problem connecting to remote hosts.

# Install Zigbee to MQTT relay on RHEL

Installs an utility that realys Zigbee to MQTT protocol into RHEL box, along with:

* Systemd control file, service enabled across reboots
* Configures the realy to connect to local MQTT broker on RHEL.

# Delete Zigbee to MQTT relay from RHEL

Removes zigbee2mqtt container from RHEL. Data is left behind.
