
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
#### Playbooks
* <code>$PDIR/esxi_vm_list.yml</code>
```yaml
---
- name: List VMs on ESXi
  hosts: esxiX
  tasks:

...
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4MDY0MjU1OTksNTk5NTQ4Mjg4LDczMD
k5ODExNl19
-->