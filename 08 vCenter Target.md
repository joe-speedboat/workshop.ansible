# vCenter Targets
Note that the examples we looked ad before are working for vCenter targets as well.

If you fire against vCenter, some VARs not used in ESXi targets become useful:
* datacenter
* folder

So lets look what we can do with VMware Targets:

    ansible-doc -l | grep vmware

Lets Create an example Playbook that creates a linked clone of a VM:

```yaml
- name: clone linked vm in vCenter
  hosts: localhost
  tasks:
    - name: Create a linked VM from snapshot
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
        linked_clone: true
        snapshot_src: "{{ item.key }}"
        hardware:
          memory_mb: "{{ item.value.mem}}"
          num_cpus: "{{ item.value.cpu}}"
          scsi: paravirtual
        networks:
        - name: "{{ network }}"
        wait_for_ip_address: no
      delegate_to: localhost
      register: deploy
      ignore_errors: true
      with_dict: "{{ machines }}"
...
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5OTExNTA0NDYsMTE2MzE5MTY0MF19
-->