
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
[esxi_hosts]
clue3

[control_nodes]
localhost
[control_nodes:vars]
esxi_user=root
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
---
- name: list all VMs on ESXi host
  hosts: localhost
  vars:
    esxi_host: esxi_hosts
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
      msg: "{{ all_guest_names }}"
...
```

##### <code>$PDIR/esxi_vm_snapshot_list.yml</code>
```yaml
---
- name: list all VM snapshots on ESXi host
  hosts: localhost
  vars:
    esxi_host: esx1
    vmname: vm1
  tasks:
  - name: Get VM Snapshots
    vmware_guest_snapshot_info:
      hostname: "{{ esxi_host }}"
      username: "{{ esxi_user }}"
      password: "{{ esxi_password }}"
      datacenter: ""
      folder: ""
      validate_certs: no
      name: "{{ vmname }}"
    register: snapshot_info
    delegate_to: localhost
  - name: print all snapshots on VM {{ vmname }}
    debug: 
      var: snapshot_info.guest_snapshots.snapshots
...
```

##### <code>$PDIR/esxi_vm_snapshot_revert.yml</code>
```yaml
---
- name: Revert into VM snapshot on ESXi host
  hosts: localhost
  vars:
    esxi_host: esx1
    vmname: vm1
    snapname: snap1
  tasks:
  - name: Revert Snapshot
    vmware_guest_snapshot:
      hostname: "{{ esxi_host }}"
      username: "{{ esxi_user }}"
      password: "{{ esxi_password }}"
      datacenter: ""
      folder: ""
      validate_certs: no
      name: "{{ vmname }}"
      state: revert
      snapshot_name: "{{ snapname }}"
      quiesce: yes
      memory_dump: yes
    delegate_to: localhost
...
```

##### <code>$PDIR/esxi_vm_power_on.yml</code>
```yaml
---
- name: PowerOn VM on ESXi host
  hosts: localhost
  vars:
    esxi_host: esx1
    vmname: vm1
  tasks:
  - name: Power on VM
    vmware_guest_powerstate:
      hostname: "{{ esxi_host }}"
      username: "{{ esxi_user }}"
      password: "{{ esxi_password }}"
      validate_certs: no
      name: "{{ vmname }}"
      state: powered-on
    delegate_to: localhost
...
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc2Mjg0MzEsLTExNjA0NDM2NzYsMTQ2Mz
k3NTY5MF19
-->