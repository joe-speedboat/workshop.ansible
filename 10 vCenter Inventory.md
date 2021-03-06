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
mkdir collections roles
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

### Hands On: vCenter Inventory Internals
Let's Explore how Ansible works with this new Inventory
```bash
ansible-inventory --host gateway1
{
    "ansible_host": "192.168.223.77",
    "config.cpuHotAddEnabled": false,
    "config.cpuHotRemoveEnabled": false,
    "config.guestId": "other3xLinux64Guest",
    "config.hardware.numCPU": 1,
    "config.instanceUuid": "52a30a58-a281-3ec4-06a6-42228caf25d1",
    "config.name": "gateway1",
    "config.template": false,
    "config.uuid": "564da15e-e7ff-6cc9-b3ad-c75164aa7c90",
    "guest.guestId": "other3xLinux64Guest",
    "guest.guestState": "running",
    "guest.hostName": "gateway1",
    "guest.ipAddress": "192.168.223.77",
    "name": "gateway1",
    "runtime.connectionState": "connected",
    "runtime.maxMemoryUsage": 128,
    "summary.runtime.powerState": "poweredOn"
}
```
Ha, not bad, so we do not only have the VM facts from now on.

So lets look deeper into this VM, which is part of a nested LAB and completely unmanaged with a static IP.
* Check DNS
```bash
host gateway1
	Host gateway1 not found: 3(NXDOMAIN)
```
Okay, no DNS that we can use for connection!?

* Check connectivity
```bash
ping -c1 -w1 192.168.223.77 >/dev/null && echo ok || echo fail
	ok

nc -z -v 192.168.223.77 22
	Ncat: Version 7.70 ( https://nmap.org/ncat )
	Ncat: Connected to 192.168.223.77:22.
	Ncat: 0 bytes sent, 0 bytes received in 0.01 seconds.
```
Seems so, so lets do a final check how Ansible would try to reach this brand new linux?!
```bash
ansible -m ping gateway1 -vvv 2>&1 |  grep EXEC
<192.168.223.77> SSH: EXEC ssh -C -o ControlMaster=auto -o ControlPersist=60s -o KbdInteractiveAuthentication=no -o PreferredAuthentications=gssapi-with-mic,gssapi-keyex,hostbased,publickey -o PasswordAuthentication=no -o 'User="root"' -o ConnectTimeout=10 -o ControlPath=/root/.ansible/cp/39a2d87d88 192.168.223.77 '/bin/sh -c '"'"'echo ~root && sleep 0'"'"''
```
* Do you see the IP?
* What does this mean?

### Review
* What is the benefit of a vCenter API Ansible inventory?
* What can you do with the new facts?
	* Power State
	* Operating System
	* Tags
	* ...


<!--stackedit_data:
eyJoaXN0b3J5IjpbNTc1NjQzMzY0LDM5NTAxNDY1Myw2NDUxNj
IyNTUsLTg1NTc4Nzk1MCwxNjk4MTc5NjE1LDIwNTI1NTA3NDUs
LTEwMzM4NjgxNTcsLTIwODU3MjYwNDMsLTI2MTUwMzEyOF19
-->