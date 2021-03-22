
# Ansible ESXi Targets
This is an example on how to execute tasks on esxi targets directly.

## Minimal Playbook Setup
### Prepare ESXi VM
* Setup a ESXi
* Setup a VM in the new ESXi

### Create Ansible Project Files

#### Project
```bash
PNAME="VMware_ESXi"
PDIR="/etc/ansible/projects/esxi_demo"
mkdir -p $PDIR
chmod 700 $PDIR
```
#### Inventory
* <code>$PDIR/inventory</code>
```ini
# ansible demo inventory for $PNAME
[esxi_hosts]
esxi1

[esxi_hosts:vars]
esxi_user=root
esxi_password=ChangeMe...
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
* <code>$PDIR/win_update.yml</code>
```yaml
---
- name: Update with Important Windows Patches
  hosts: vm12
  tasks:
  - name: Apply Patches
    win_updates:
      category_name:
      - SecurityUpdates
      - CriticalUpdates
      blacklist:
      - Windows Malicious Software Removal Tool for Windows
      - \d{4}-\d{2} Cumulative Update for Windows Server 2016
      reboot: yes
      reboot_timeout: 1800
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
eyJoaXN0b3J5IjpbNTk5NTQ4Mjg4LDczMDk5ODExNl19
-->