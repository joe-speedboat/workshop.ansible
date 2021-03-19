
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
## Ansible Config File

### Project (project_folder/ansible.cfg)

### User ($HOME/.ansible.cfg)
We don't need that in our labs, but if existing, this would override the system configuration.

### System (/etc/ansible/ansible.cfg)
We already created that during the setup.
Let's check what's configured there:
```
egrep -v '^$|^#' /etc/ansible/ansible.cfg 
	[defaults]
	inventory      = /etc/ansible/hosts
	roles_path    = /etc/ansible/roles
	collections_paths = /etc/ansible/collections
	remote_user = root
	log_path = /var/log/ansible.log
	nocows = 1
	[inventory]
	[privilege_escalation]
	[paramiko_connection]
	[ssh_connection]
	[persistent_connection]
	[accelerate]
	[selinux]
	[colors]
	[diff]
```

## inventory
We not what an inventory is, but let us create a first project with a custom inventory in it:
```bash
mkdir /etc/ansible/projects/os_updates
cd /etc/ansible/projects/os_updates

echo '# custom ansible project configuration
[defaults]
inventory      = ./inventory
roles_path    = ./roles
collections_paths = ./collections
remote_user = root
log_path = ./ansible.log
' > ansible.cfg
```
```
mkdir ./collections ./roles
```
```
echo '
[ansibleserver]
ansible.office.bitbull.ch

[testserver]
vm[01:40]

[nextcloud]
cloud1

[kvm]
clue1
clue2
saturn.bitbull.ch ansible_ssh_port=222 ospatch_reboot=false

[esxi]
clue3 gather_facts=False

[windows]
win[01:10]

[office]
vpn
lts1
install
domoticz.office.bitbull.ch
ogate.office.bitbull.ch
graylog.office.bitbull.ch
name.office.bitbull.ch
sso.office.bitbull.ch
cloud.office.bitbull.ch

[wan]
rigel.bitbull.ch ansible_ssh_port=50022 ospatch_reboot=false
pan.bitbull.ch ansible_ssh_port=22 ospatch_reboot=false

gate.bitbull.ch ansible_ssh_port=222

' > ansible.cfg
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM1NzUxMTcxMSwtMTY4NzY4OTE3NywtOT
E4MzkzNjA5XX0=
-->