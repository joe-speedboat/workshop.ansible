
# Ansible Files
With Ansible you can work in three different contexts.
This depends on where you place your Ansible configuration.

To check which config file is used, you can ask `ansible`:
```
ansible --version
	ansible 2.9.18
	  config file = /etc/ansible/ansible.cfg
	  configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
	  ansible python module location = /usr/lib/python3.6/site-packages/ansible
	  executable location = /usr/bin/ansible
	  python version = 3.6.8 (default, Aug 24 2020, 17:57:11) [GCC 8.3.1 20191121 (Red Hat 8.3.1-5)]
```


## Project (project_folder/ansible.cfg)

## User ($HOME/.ansible.cfg)
We don't need that in our labs, but if existing, this would override the system configuration.

## System (/etc/ansible/ansible.cfg)
We already created that during the setup.
Let's check what's configured there:


#### Ansible Config File

#### inventory



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY0OTUwMDI1NCwtOTE4MzkzNjA5XX0=
-->