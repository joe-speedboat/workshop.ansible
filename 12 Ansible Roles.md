# Ansible Roles
We have seen roles before.
Can you Explain to me how it works?

## Explore Roles

### Project
```bash
PNAME="os_update_role"
PDIR="/etc/ansible/projects/demo_role"
mkdir -p $PDIR
chmod 700 $PDIR
cd $PDIR
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

### Inventory
* <code>$PDIR/inventory</code>
```ini
# ansible demo inventory for $PNAME
[linux]
vm01
vm02

[windows]
vm03
vm04
```

### Prepare Windows config
* Enable and Configure WinRM
* Provide Credentials on Control node
```bash
mkdir group_vars
vim group_vars/windows.yml
```
```
ansible_user: Administrator
ansible_password: changeMe...
ansible_connection: winrm
ansible_winrm_server_cert_validation: ignore
```

### Prepare Linux Nodes
* Create and install ssh-key
```bash
test -f /root/.ssh/id_*.pub || ssh-keygen
ssh-copy-id vm01
ssh-copy-id vm02
```

### Test config & connectivity
```
ansible -m ping linux
ansible -m win_ping windows
```

### Install and explore roles
```bash
mkdir $PDIR/roles
cd $PDIR/roles
ansible-galaxy init demo.ping
find demo.ping
cd ..
cat roles/demo.ping/tests/test.yml
ansible-playbook roles/demo.ping/tests/test.yml
```
Explore the role elements with online documentation:

https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html
Can you explain what it is and what's the idea behind roles?

## Write a first Role
Now we have created an empty role, so lets do something with it.
```bash
ansible -m setup windows | grep ansible_system
ansible -m setup linux | grep ansible_system

* <code>$PDIR/roles/demo.ping/tasks/main.yml</code>
```yaml
---
# tasks file for demo.ping
- name: ping linux host
  ping:
  when: ansible_system == "Linux"

- name: ping windows host
  win_ping:
  when: ansible_system == "Win32NT"
...
```
### Test the role
```bash
cp roles/demo.ping/tests/test.yml 


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIzNDAzMDQyOSwtNDEzNjg3NTUzLDM2ND
A1NDY5LC0xNTI5NDgyNjc0XX0=
-->