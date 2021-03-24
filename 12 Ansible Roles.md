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

<!--stackedit_data:
eyJoaXN0b3J5IjpbNTk4OTg3NTU0XX0=
-->