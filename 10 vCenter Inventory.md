# Ansible vCenter Inventory
Since Inventory brings the Ansible targets to the playbooks, Inventory should always be up to date.
Ansible would not be that popular if it would not bring a solution that brings ZEN to the inventory by calling the vCenter API.

https://docs.ansible.com/ansible/latest/scenario_guides/vmware_scenarios/vmware_inventory.html

## Setup vCenter Ansible Inventory

### Install Python Libs
VMware modules mostly work with pyVim.
Install it with:

    pip3 install pyVim pyVmomi

### Project
```bash
PNAME="VMware_vCenter_inventory"
PDIR="/etc/ansible/projects/vcenter_inventory"
mkdir -p $PDIR
chmod 700 $PDIR
```
### Inventory
* <code>$PDIR/inventory</code>
```ini
# ansible demo inventory for $PNAME
[vcvms]
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
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNjgyNjIxMzhdfQ==
-->