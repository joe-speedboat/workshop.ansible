
# Cisco Switch Targets
Since Ansible can configure lot of target types, we look at Cisco devices now.
The only tricky thing is to get a working example config, but lets create one.

### Create Ansible Project Files

#### Project
```bash
PNAME="Cisco_Switch"
PDIR="/etc/ansible/projects/cisco_switch/back"
mkdir -p $PDIR
chmod 700 $PDIR
```
#### Inventory
* <code>$PDIR/inventory</code>
```ini
# ansible demo inventory for $PNAME
[switch]
switch1
```
#### Ansible Config
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
#### Playbook
* <code>$PDIR/win_update.yml</code>
```yaml
---

...
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA3NDE2NjAxM119
-->