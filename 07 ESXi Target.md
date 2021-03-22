
# Ansible ESXi Targets
This is an example on how to execute tasks on esxi targets directly.

## Prepare ESXi VM
* Setup a ESXi
* Setup a VM in the new ESXi

## Create Ansible Project Files

### Project
```bash
PNAME="VMware_ESXi"
PDIR="/etc/ansible/projects/esxi_demo"
mkdir -p $PDIR
chmod 700 $PDIR
```
### Inventory
* <code>$PDIR/inventory</code>
```ini
# ansible demo inventory for $PNAME
[esxi_hosts]
esxi1

[esxi_hosts:vars]
esxi_user=root
esxi_password=ChangeMe...
```
### Ansible Config
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
### Playbooks
##### <code>$PDIR/esxi_vm_list.yml</code>
```yaml
---
- name: list all VMs on esxi host
  hosts: localhost
  vars:
    esxi_host: esxi1
  tasks:
  - name: loop virtual machines

block:

- set_fact:

all_guest_names: []

- name: Get virtual machine names

vmware_vm_info:

hostname: "{{ esxi_host }}"

username: "{{ esxi_user }}"

password: "{{ esxi_password }}"

folder: ""

validate_certs: no

delegate_to: localhost

register: vm_info

- set_fact:

all_guest_names: "{{ all_guest_names + [ item.guest_name ] }}"

no_log: true

with_items:

- "{{ vm_info.virtual_machines | json_query(query) }}"

vars:

query: "[?guest_name]"

  

- name: print all virtual machine names

debug:

msg: "{{ all_guest_names }}"...
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQyNDE1OTIxMCwyMTE5MDEyMjk2LDU5OT
U0ODI4OCw3MzA5OTgxMTZdfQ==
-->