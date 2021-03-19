# Getting Started
Now we have installed Ansible on the Linux VM, lets see which commands we can use
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

## Ansible commands

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
With the ansible command, we can define and run a single task 'playbook' against a set of hosts.
Localhost is a target that is builtin, so we will update the localhosts software with ansible as an example exercise.
* First we look at the documentation
```bash
ansible-doc dnf
```

* Now we just update the software with an ad-hoc ansible command:
First let us check if Ansible can connect the target server
```
ansible -m ping localhost

	localhost | SUCCESS => {
	    "changed": false,
	    "ping": "pong"
	}
```
```
ansible -m dnf -e state=latest -e name='*' localhost

	localhost | SUCCESS => {
	    "changed": false,
	    "msg": "Nothing to do",
	    "rc": 0,
	    "results": []
	}
```


### ansible-galaxy
Is a command to manage Ansible roles in shared repositories, the default of which is Ansible Galaxy _https://galaxy.ansible.com_.

Lets see if we can find some content from [uniQconsulting ag](https://www.uniqconsulting.ch)?
```
ansible-galaxy search uniqconsulting

	Found 20 roles matching your search:
	 Name                            Description
	 ----                            -----------
	 uniqconsulting.collabora        installation and configuration of collabora CODE with nginx reverse proxy
	 uniqconsulting.duo              Duo Auth Proxy
	 uniqconsulting.elasticsearch    Installs and configures a basic single-instanc elasticsearch server
	 uniqconsulting.firewall         Set's up a firewall with Open-Ports, Port-Forwarding & SNAT
	 uniqconsulting.graylog          Install graylog and configure single server instance
	 uniqconsulting.graylog3         Install graylog3 and configure single server instance
	 uniqconsulting.guacamole        Install and configure Guacamole with Nginx ssl reverse proxy
	 uniqconsulting.iptables         iptables configuration
	 uniqconsulting.mariadb          installation and configuration of mariadb (mysql)
	 uniqconsulting.mongodb          Installs and configures MongoDB
	 uniqconsulting.network_setup    Network Setup
	 uniqconsulting.nextcloud        nextcloud installation and configuration
	 uniqconsulting.nginx            Installs/Configures Nginx for Webserver & Proxy use
	 uniqconsulting.open_vm_tools    installation of open VMware Tools
	 uniqconsulting.os_basic         Basic OS Server Setup
	 uniqconsulting.os_update        Full or Security OS Updates
	 uniqconsulting.otrs             otrs installation and configuration
	 uniqconsulting.php              Installs and Configures php-fpm
	 uniqconsulting.phpipam          phpIPAM
	 uniqconsulting.veeam_linux_repo installation and configuration of Veeam Repository
```
Wow, great. Did you know about this content!

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
eyJoaXN0b3J5IjpbLTExMDUwOTE3MDksOTI3NDM0MDUxLC0yMT
E1NjYwMzM3LDY4MTM0NzQyMCwyMDM1OTY1NzM4XX0=
-->