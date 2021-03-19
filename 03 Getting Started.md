# Getting Started
Now we have installed Ansible on the Linux VM, lets see which comands we can use
```
rpm -ql ansible | grep bin/
	/usr/bin/ansible
	/usr/bin/ansible-config
	/usr/bin/ansible-connection
	/usr/bin/ansible-console
	/usr/bin/ansible-doc
	/usr/bin/ansible-galaxy
	/usr/bin/ansible-inventory
	/usr/bin/ansible-playbook
	/usr/bin/ansible-pull
	/usr/bin/ansible-test
	/usr/bin/ansible-vault
```

## Ansible Comands

### ansible-doc
This is the plugin documentation tool. 
Let's explore what this can do for us:
* Show all plugins
```
ansible-doc -l
```
* Count the amount of modules a standard Ansible installation provides to us?
```
ansible-doc -l | wc -l
```
Oh wow, about 3400 modules!
We can do a lot of things with that, but how?

### ansible
With the ansible comand, we can define and run a single task 'playbook' against a set of hosts.
Localhost is a target that is builtin, so we will update the localhosts software with ansible as an example exercise.
* First we look at the documentation
```{r, engine='bash'}
ansible-doc dnf
```
* Now we just update the software with an ad-hoc ansible comand:
First let us check if Ansible can connect the target server
```bash
ansible -m ping localhost
```
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
</span>




ansible-galaxy
ansible-inventory
ansible-playbook
ansible-test
ansible-vault



### Ansible Files
#### cfg
#### inventory
#### playbook




### Ansible Projects
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjYyMDI5OTc0LDY4MTM0NzQyMCwyMDM1OT
Y1NzM4XX0=
-->