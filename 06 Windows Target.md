# Ansible Windows Targets
For sure you can reach Windows Systems as well.
* Do you remember how this works?
	* Explain it to the class
* What prerequisites do we need to cover?

## Minimal Windows Playbook Setup
### Prepare Windows VM
* Setup a current Windows OS
* Create local admin user
* Configure WinRM

### Create Ansible Project Files

#### Project
```bash
PNAME="Windows_WinRM"
PDIR="/etc/ansible/projects/win_demo"
mkdir -p $PDIR
chmod 700 $PDIR
```
#### Inventory
```bash
echo "# ansible demo inventory for $PNAME
[all:vars]
ansible_connection: winrm
ansible_user: loc_adm
ansible_password: ChangeMe...

[win_demo]
vm12
" > $PDIR/inventory
```
#### Ansible Config
```bash
echo "# custom ansible $PNAME configuration
[defaults]
inventory      = ./inventory
roles_path    = ./roles
collections_paths = ./collections
remote_user = root
log_path = ./ansible.log
" > $PDIR/ansible.cfg
```
#### Playbook


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU3OTg3MDUzOSwtMTk4MDU5NDgwOF19
-->