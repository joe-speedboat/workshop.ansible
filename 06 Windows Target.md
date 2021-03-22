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

### Create Ansible Project 
```bash
PNAME="Windows_WinRM"
PDIR="/etc/ansible/projects/win_demo"
mkdir -p $PDIR
chmod 700 $PDIR
echo "# ansible demo inventory for  $PNAME
[all:vars]
ansible_connection: winrm
ansible_user: administrator
ansible_password: This-Should-Be-a-Password!
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNDk5NjM5NTcsLTE5ODA1OTQ4MDhdfQ
==
-->