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
ansible -m dnf -e state=latest -e name='*' -e update_cache=true localhost

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
Wow, great. 
Did you know about this content?!
But enough for now, let's look into roles later!

### ansible-inventory
We already discussed about inventory. With this command, we can show the content of an inventory in a useful way.
As an example we can use this comand, but since we did not define an inventory so far, it will not show that much:
```bash
ansible-inventory --list --vars --yaml
```

### ansible-playbook
We know about playbooks, so lets create a first one which is doing exactly the same as ad-hoc comand above did.
```bash
vim /etc/ansible/update_localhost.yml
```
```yml
---
- name: update a centos 8 system
  hosts: localhost
  tasks:
    - name: update the system with dnf module
      dnf:
        name: '*'
        state: latest
        update_cache: true
...
```
Okay, lets start this one

    ansible-playbook /etc/ansible/update_localhost.yml
```
PLAY [update a centos 8 system] *****************************************************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************************************************************
ok: [localhost]

TASK [update the system with dnf module] ********************************************************************************************************************************************************
ok: [localhost]

PLAY RECAP **************************************************************************************************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

```
Okay, fine.
Please note the **changed=0** in the output.
This means that noting has been changed at all, the system got not modified at all.

### ansible-vault
Is an encryption/decryption utility for Ansible data files.
We need this to protect sensible data like passwords for our work.
So lets create our first vault:
```
ansible-vault create /etc/ansible/passwords.yml
	New Vault password: 
	Confirm New Vault password: 
```
Please remember the password, we need it later on!
Now lets fill the secrets into the vault:
```
---
mypass: superSecret!
...
```
okay, let's see whats in the file:
```
cat /etc/ansible/passwords.yml
	$ANSIBLE_VAULT;1.1;AES256
	64383234656538303334363035303639343039666133313436633163656238666339303031356665
	3962633636313939323862396539383861363236316135360a323162636532366332333461643434
	65303539626434383132653961333061313331306631636533396632646263313436306132646238
	6361383430376532350a613533663237366430346632373265356139323438333662376236636438
	32303561613933663530353939623835643936393166646366646631633732356531
```
And now let's decrypt the vault:
```
ansible-vault view /etc/ansible/passwords.yml
Vault password: 
```
```
---
password: superSecret!
...
```





<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU4NDA2MjI1MiwtMTI0Njc2MzY3NSwtMT
UwNjQ0MjE0MCwxNDIwNzk3OTk0LC0yMDA2OTMwMDE5LC0xMzA0
NDkxMzI4LC0xNjYzNzAyMzE3LDkyNzQzNDA1MSwtMjExNTY2MD
MzNyw2ODEzNDc0MjAsMjAzNTk2NTczOF19
-->