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

### Prepare Windows config
* Enable and Configure WinRM
* Provide Credentials on Control node
```bash
mkdir group_vars/windows.yml
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
ssh-copy-id vm01
```

### Test config & connectivity
```
ansible -m ping linux
ansible -m win_ping windows
```

### Install and explore roles
```bash
cd $PDIR/roles
ansible-galaxy init demo.ping
find demo.ping
```





ansible-galaxy install sbaerlocher.update_management
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc1MjY4MTg0LC0xNTI5NDgyNjc0XX0=
-->