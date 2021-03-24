# Ansible vCenter Inventory
Since Inventory brings the Ansible targets to the playbooks, Inventory should always be up to date.
Ansible would not be that popular if it would not bring ZEN to the inventory by calling the vCenter API.

https://galaxy.ansible.com/community/vmware
https://cloudautomation.pharriso.co.uk/post/vmware_filter_tags/



## Setup vCenter Ansible Inventory

### Install Python Libs
VMware modules mostly work with pyVim.
Install it with:

```bash
pip3 install --upgrade pip setuptools
pip3 install --upgrade git+https://github.com/vmware/vsphere-automation-sdk-python.git
pip3 install --upgrade pyVim pyVmomi
```

### Project
```bash
PNAME="VMware_vCenter_inventory"
PDIR="/etc/ansible/projects/vcenter_inventory"
mkdir -p $PDIR
chmod 700 $PDIR
cd $PDIR
```
### Inventory
* <code>$PDIR/inventory.vmware.yml</code>
```ini
plugin: community.vmware.vmware_vm_inventory
strict: False
hostname: 10.65.223.31
username: administrator@vsphere.local
password: Esxi@123$%
validate_certs: False
with_tags: True
hostnames:
  - config.name
```

### Ansible Config
* <code>$PDIR/ansible.cfg</code>
```ini
# custom ansible $PNAME configuration
[defaults]
inventory      = ./inventory.vmware.yml
roles_path    = ./roles
collections_paths = ./collections
remote_user = root
log_path = ./ansible.log
[inventory]
enable_plugins = community.vmware.vmware_vm_inventory
```

### Install VMware Community Collections

    ansible-galaxy collection install community.vmware

### Query the new Inventory

    ansible-inventory --list --vars

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA1MjU1MDc0NSwtMTAzMzg2ODE1NywtMj
A4NTcyNjA0MywtMjYxNTAzMTI4XX0=
-->