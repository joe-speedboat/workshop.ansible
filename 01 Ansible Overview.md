# Ansible Overview
![enter image description here](https://github.com/joe-speedboat/workshop.ansible/raw/master/images/ansible_logo.png)

![enter image description here](https://github.com/joe-speedboat/workshop.ansible/raw/master/images/devops.png)



## History of Ansible
* Ansible was written by Michael DeHaan in 20212
* Acquired by Red Hat in 2015

## Design
* **Minimal in nature**
Management systems should not impose additional dependencies on the environment.

* **Consistent**
With Ansible one should be able to create consistent environments.

* **Secure**
Ansible does not deploy agents to nodes. 
Only OpenSSH and Python are required on the managed nodes.
Windows systems use WinRM ans PowerShell.

* **Highly Reliable**
When carefully written, an Ansible playbook can be idempotent, to prevent unexpected sideeffects on the managed systems.
It is entirely possible to have a poorly written playbook that is not idempotent.
	* Idempotent
On/Off buttons of a train's destination sign control panel. Pressing the On button (green) is an idempotent operation, since it has the same effect whether done once or multiple times. Likewise, pressing Off is idempotent.

* **Minimal learning required**
Playbooks use an easy and descriptive language based on YAML and Jinja templates.

## Architecture

![enter image description here](https://github.com/joe-speedboat/workshop.ansible/raw/master/images/ansible_architecture.png)
### [Inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)
Ansible works against multiple managed nodes or “hosts” in your infrastructure at the same time, using a list or group of lists known as inventory. Once your inventory is defined, you use [patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#intro-patterns) to select the hosts or groups you want Ansible to run against.

### [Facts](https://docs.ansible.com/ansible/latest/user_guide/playbooks_vars_facts.html)
With Ansible you can retrieve or discover certain variables containing information about your remote systems or about Ansible itself. Variables related to remote systems are called facts. With facts, you can use the behavior or state of one system as configuration on other systems. For example, you can use the IP address of one system as a configuration value on another system. Variables related to Ansible are called magic variables.

### Variables

### Playbooks

### Roles
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjE5MDM5MTU3XX0=
-->