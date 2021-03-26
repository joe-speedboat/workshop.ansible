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
Windows systems use WinRM and PowerShell.

* **Highly Reliable**
When carefully written, an Ansible playbook can be idempotent, to prevent unexpected sideeffects on the managed systems.
It is entirely possible to have a poorly written playbook that is not idempotent.
	* Idempotent
On/Off buttons of a train's destination sign control panel. Pressing the On button (green) is an idempotent operation, since it has the same effect whether done once or multiple times. Likewise, pressing Off is idempotent.

* **Minimal learning required**
Playbooks use an easy and descriptive language based on YAML and Jinja templates.


## Architecture

![enter image description here](https://github.com/joe-speedboat/workshop.ansible/raw/master/images/ansible_architecture.png)

![enter image description here](https://github.com/joe-speedboat/workshop.ansible/raw/master/images/ansible_topo1.png)
![enter image description here](https://github.com/joe-speedboat/workshop.ansible/raw/master/images/ansible_topo2.png)

### [Inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)
Ansible works against multiple managed nodes or “hosts” in your infrastructure at the same time, using a list or group of lists known as inventory. Once your inventory is defined, you use [patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#intro-patterns) to select the hosts or groups you want Ansible to run against.


### [Facts](https://docs.ansible.com/ansible/latest/user_guide/playbooks_vars_facts.html)
With Ansible you can retrieve or discover certain variables containing information about your remote systems or about Ansible itself. Variables related to remote systems are called facts. With facts, you can use the behavior or state of one system as configuration on other systems. For example, you can use the IP address of one system as a configuration value on another system. Variables related to Ansible are called magic variables.


### [Variables](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html)
Ansible uses variables to manage differences between systems. With Ansible, you can execute tasks and playbooks on multiple different systems with a single command. To represent the variations among those different systems, you can create variables with standard YAML syntax, including lists and dictionaries. You can define these variables in your playbooks, in your [inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#intro-inventory), in re-usable [files](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse.html#playbooks-reuse) or [roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html#playbooks-reuse-roles), or at the command line. You can also create variables during a playbook run by registering the return value or values of a task as a new variable.


### [Playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html)
Ansible Playbooks offer a repeatable, re-usable, simple configuration management and multi-machine deployment system, one that is well suited to deploying complex applications. If you need to execute a task with Ansible more than once, write a playbook and put it under source control. Then you can use the playbook to push out new configuration or confirm the configuration of remote systems. The playbooks in the [ansible-examples repository](https://github.com/ansible/ansible-examples) illustrate many useful techniques.


### [Roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html)
Roles let you automatically load related vars_files, tasks, handlers, and other Ansible artifacts based on a known file structure. Once you group your content in roles, you can easily reuse them and share them with other users.
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDgyODc1NjYxLC04MDg5MjI2MDAsOTQyNj
U3MDAxLC02OTQ2NTIxODFdfQ==
-->