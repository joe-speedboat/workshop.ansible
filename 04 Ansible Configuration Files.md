
# Ansible Configuration Files
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
```bash
mkdir ./collections ./roles
```
```bash
echo "
[all]
[all:vars]
ntp_srv='pool.ntp.org'

[grp_ch_kvm]
kvm1
kvm2 
[grp_ch_kvm:vars]
ospatch_reboot=false

[grp_ch_win]
win[01:02]

[grp_ch_lin]
graylog
nfs1
lin[01:02]

[grp_os_lin]
[grp_os_lin:children]
grp_ch_lin
grp_ch_kvm

[grp_os_win]
[grp_os_win:children]
grp_ch_win

[loc_ch]
[loc_ch:children]
grp_ch_win
grp_ch_lin
grp_ch_kvm

[loc_ch:vars]
dns_srv=['192.168.100.254', '192.168.100.253']
domain='ch.domain.com'
ntp_srv='ch.pool.ntp.org'
syslog_srv='syslog.domain.com'
" > inventory
```
Not lets check how Ansible looks into that inventory file:
```bash
ansible-inventory --graph --vars --yaml
ansible-inventory --list --vars --yaml
ansible-inventory --vars --yaml --host kvm1
```

Do you understand how vars and children got handled in the inventory?
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ3ODY3MDE5MywtNjU5ODc0OTgyLDExNj
E2Njg5NjQsLTE2ODc2ODkxNzcsLTkxODM5MzYwOV19
-->