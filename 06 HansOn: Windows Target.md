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
	* https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html 

### Create Ansible Project Files

#### Project
```bash
PNAME="Windows_WinRM"
PDIR="/etc/ansible/projects/win_demo"
mkdir -p $PDIR
chmod 700 $PDIR
```
#### Inventory
* <code>$PDIR/inventory</code>
```yaml
# ansible demo inventory for $PNAME
[win_hosts]
vm12

[win_hosts:vars]
ansible_connection: winrm
ansible_user: loc_adm
ansible_password: ChangeMe...
```
#### Ansible Config
* <code>$PDIR/ansible.cfg</code>
```ini
# custom ansible $PNAME configuration
[defaults]
inventory      = ./inventory
roles_path    = ./roles
collections_paths = ./collections
remote_user = root
log_path = ./ansible.log
```
#### Playbook
```yaml
---
- name: Run Updates on Windows Servers
  hosts: win_hosts
  connection: winrm

  tasks:
    win_updates:
      category_names:
        - CriticalUpdates
        - SecurityUpdates
      blacklist:
        - Microsoft Silverlight
    reboot: yes
    reboot_timeout: 900
...
```

#### Run the Update
Just do it and look what happens on the Windows Hosts Console.

### Hints
* Shell Ping?
* WinRM?
* Ansible Ping?
* Vars?
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjEzMjM1NTQ5LDE3NTIwMTE1NzFdfQ==
-->