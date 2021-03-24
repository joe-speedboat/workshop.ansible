# Ansible Roles
We have seen roles before.
Can you Explain to me how it works?

## Explore Roles

### Project
```bash
PNAME="os_update_role"
PDIR="/etc/ansible/projects/cisco_switch"
mkdir -p $PDIR
chmod 700 $PDIR
cd $PDIR
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

### Prepare Windows Nodes
* Enable and Configure WinRM
* Provide Credentials on Control node
```bash
mkdir group_vars/windows.yml
ansible-vault create group_vars/windows.yml
```
```
ansible_user: Administrator
ansible_password: changeMe...
ansible_connection: winrm
ansible_winrm_server_cert_validation: ignore
```

### Prepare Linux Nodes
* Create and
test -f /root/.ssh/id_*.pub || ssh-keygen

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA0MzY0NzAwM119
-->