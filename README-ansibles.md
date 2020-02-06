# Ansible Tower automation

This doc describes Ansible automation implemented into Tower for the project. There is vault file used in the project for the secrets, and some of the secrets are in Tower instance inventories, also vault encrypted. The following list of templates is available at the Tower:

# Setup RHEL Basics

This task is meant to make sure the remote RHEL device is set up properly for the exercise. It runs [setup-rhel-host playbook](https://github.com/RedHatNordicsSA/iot-hack/blob/master/setup-rhel-host.yml). The following tasks are done:

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

