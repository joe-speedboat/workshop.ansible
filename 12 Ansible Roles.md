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
[cisco_switch]
switch1
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ5NDc3MDY2Nl19
-->