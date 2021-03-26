
# Exercise vCenter Linked Clone
In this exercise you have to write a playbook.

## Create Ansible Project Files

### Install Python Libs
VMware modules mostly work with pyVim.
Install it with:

    pip3 install pyVim pyVmomi

### Project
```bash
PNAME="VMware_vCenter"
PDIR="/etc/ansible/projects/vcenter_demo"
mkdir -p $PDIR
chmod 700 $PDIR
cd $PDIR
```

### Inventory
* <code>$PDIR/inventory</code>
```ini
# ansible demo inventory for $PNAME
[vcenter_hosts]
vcenter01

[control_nodes]
localhost
[control_nodes:vars]
esxi_user=ansible@vsphere.local
esxi_password=changeME...
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

```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg5MzczOTEyLC0xMTYwNDQzNjc2LDE0Nj
M5NzU2OTBdfQ==
-->