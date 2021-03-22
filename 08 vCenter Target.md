# vCenter Targets
Note that the examples we looked ad before are working for vCenter targets as well.

If you fire against vCenter, some VARs not used in ESXi targets become useful:
* datacenter
* folder

So lets look what we can do with VMware Targets:

    ansible-doc -l | grep vmware

## Example
Lets Create an example Playbook that creates a linked clone of a VM:

```yaml
- name: clone VM in vCenter
  hosts: localhost
  tasks:
    - name: Create a VM from Template
      vmware_guest:
        hostname: vcenter1
        username: administrator@vsphere.local
        password: ChangeMe...
        validate_certs: no
        datacenter: BITBULL
        cluster: LNX
        folder: /Ansible
        name: clone1
        state: poweredoff
        template: LNX_TEMPLATE
        hardware:
          memory_mb: "1024
          num_cpus: 2
          scsi: paravirtual
        networks:
        - name: DMZ
        wait_for_ip_address: no
      delegate_to: localhost
      register: deploy
      ignore_errors: true
...
```



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NjExMzc5ODAsMTM3NzU2MjU2LDExNj
MxOTE2NDBdfQ==
-->