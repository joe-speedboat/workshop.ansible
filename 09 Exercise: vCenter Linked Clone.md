
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
##### <code>$PDIR/vcenter_create_linked_clone.yml</code>
```yaml
- name: clone linked VM in vCenter
  hosts: localhost
  vars:
    vcenter_host: vcenter01
    vcenter_user: "{{ esxi_user }}"
    vcenter_password: "{{ esxi_password }}"
    template: ALPINE
    datacenter: OFFICE
    cluster: ""
    vmfolder: /Ansible
    network: "VM Network"
    snapname: snap1
    clone_name: linkclone1
    clone_cpu: 1
    clone_mem: 256
    
  tasks:
    - name: Create a linked Clone VM from Template
      vmware_guest:
        hostname: "{{ vcenter_host }}"
        username: "{{ vcenter_user }}"
        password: "{{ vcenter_password }}"
        validate_certs: no
        datacenter: "{{ datacenter }}"
        cluster: "{{ cluster }}"
        folder: "{{ vmfolder }}"
        state: poweredoff
        template: "{{ template }}"
        linked_clone: true
        name: "{{ clone_name }}"
        snapshot_src: "{{ snapname }}"
        hardware:
          memory_mb: "{{ clone_mem }}"
          num_cpus: "{{ clone_cpu }}"
          scsi: paravirtual
        networks:
        - name: "{{ network }}"
        wait_for_ip_address: no
      delegate_to: localhost
      ignore_errors: true
...
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzE4MjI3OTY3LC0xMTYwNDQzNjc2LDE0Nj
M5NzU2OTBdfQ==
-->