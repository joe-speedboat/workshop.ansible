# Protect sensible variables
You can imagine that some of the variables may need some protection.
There is a built-in mechanic to protect variable called <code>ansible-vault<code>
You can use it in two ways by default

## ansible-vault
With ansible-vault we can create symetric encrypted files ansible can work with.
But  let's do the learning with an example

## Setup Vault Project
```bash
PNAME="AnsibleVault"
PDIR="/etc/ansible/projects/vault_demo"
mkdir -p $PDIR
chmod 700 $PDIR
cd $PDIR
```

### Inventory
* <code>$PDIR/inventory</code>
```ini
# ansible demo inventory for $PNAME
[vault_demo]
host1
```

## The ugly way
### Ansible Config
<code>$PDIR/ansible.cfg</code>
```ini
# custom ansible $PNAME configuration
[defaults]
inventory      = ./inventory
roles_path    = ./roles
collections_paths = ./collections
remote_user = root
log_path = ./ansible.log
vault_password_file = ./vault_unlock
```

### Write down the vault handler password
    echo tryNotToDo > $PDIR/vault_unlock

### Create the password vault
```bash
mkdir $PDIR/group_vars
```
```bash
ansible-vault create $PDIR/group_vars/vault_demo.yml
```
```yaml
---
password: superSecretStuff...
...
```

### Explore how it works
```bash
cat vault_unlock
cat group_vars/vault_demo.yml
ansible-inventory --vars --list
```
Do you understand why I call it the ugly way?
Explain it to me!

## The uncomfortable way
### Ansible Config
<code>$PDIR/ansible.cfg</code>
```ini
# custom ansible $PNAME configuration
[defaults]
inventory      = ./inventory
roles_path    = ./roles
collections_paths = ./collections
remote_user = root
log_path = ./ansible.log
# vault_password_file = ./vault_unlock
```

### Try it out
```bash
ansible-inventory --vars --list
	ERROR! Attempting to decrypt but no vault secrets found
```
Okay, that's not what we expected.
Maybe we did something wrong
```bash
ansible-inventory --help
ansible-inventory --vars --list --ask-vault-pass
```
Can you imagine why I don't like this way either?

## The configuration I like
If you read the documentation carefully, you will find out that inventory and group_vars may be executeables as well.
Hmm, maybe this is the way it can get nicer!?
Let's try

### Ansible Config
<code>$PDIR/ansible.cfg</code>
```ini
# custom ansible $PNAME configuration
[defaults]
inventory      = ./inventory
roles_path    = ./roles
collections_paths = ./collections
remote_user = root
log_path = ./ansible.log
vault_password_file = ./vault_unlock
```
### Replace the vault password with a script

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkxOTkwNzcyMl19
-->