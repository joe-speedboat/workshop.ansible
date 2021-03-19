# Ansible HandsOn Workshop

![enter image description here](https://github.com/joe-speedboat/workshop.ansible/raw/master/images/ansible_logo.png)

![enter image description here](https://github.com/joe-speedboat/workshop.ansible/raw/master/images/devops.png)



## History of Ansible
* Ansible was written by Michael DeHaan in 20212
* Acquired by Red Hat in 2015

## Design
* **Minimal in nature**
Management systems should not impose additional dependencies on the environment.

* **Consistent**
With Ansible one should be able to create consistent environments.

* **Secure**
Ansible does not deploy agents to nodes. 
Only OpenSSH and Python are required on the managed nodes.
Windows systems use WinRM ans PowerShell.

* **Highly Reliable**
When carefully written, an Ansible playbook can be idempotent, to prevent unexpected sideeffects on the managed systems.
It is entirely possible to have a poorly written playbook that is not idempotent.
	* Idempotent
On/Off buttons of a train's destination sign control panel. Pressing the On button (green) is an idempotent operation, since it has the same effect whether done once or multiple times. Likewise, pressing Off is idempotent.

* **Minimal learning required**
Playbooks use an easy and descriptive language based on YAML and Jinja templates.

## Architecture

![enter image description here](https://github.com/joe-speedboat/workshop.ansible/raw/master/images/ansible_architecture.png)
### Ansible Setup
To install Ansible, we need a CentOS 8 or Red Hat Enterprise Linux 8 vm.
Log into the vm with ssh as root and install Ansible:

    dnf -y install epel-release
    dnf -y install git wget curl ansible vim-enhanced nano colordiff
  
   We need some new directories too

    test -d /etc/ansible/projects || mkdir /etc/ansible/projects ; chmod 700 /etc/ansible/projects
    test -d /etc/ansible/collections || mkdir /etc/ansible/collections ; chmod 755 /etc/ansible/collections
    
Now we do some basic configuration to suite our needs


    ansibleconfigfile="/etc/ansible/ansible.cfg"
    test -f ${ansibleconfigfile}.orig || cp -av $ansibleconfigfile ${ansibleconfigfile}.orig
    sed -i 's|^#inventory .*|inventory      = /etc/ansible/hosts|g' $ansibleconfigfile
    sed -i 's|^#roles_path .*|roles_path    = /etc/ansible/roles|g' $ansibleconfigfile
    sed -i 's|^#remote_user .*|remote_user = root|g' $ansibleconfigfile
    sed -i 's|^#log_path .*|log_path = /var/log/ansible.log|g' $ansibleconfigfile
    sed -i 's|^#nocows .*|nocows = 1|g' $ansibleconfigfile
    sed -i "/^roles_path/a\ \n#additional paths to search for collections in, colon separated\ncollections_paths = /etc/ansible/collections" $ansibleconfigfile

Now we look for the changes we made above:
* To the directories
```
ls -ld /etc/ansible/collections /etc/ansible/projects
  drwxr-xr-x. 2 root root 6 Mar 19 11:59 /etc/ansible/collections
  drwx------. 2 root root 6 Mar 19 11:59 /etc/ansible/projects
```
* To the config file
```
colordiff -yW"`tput cols`" $ansibleconfigfile ${ansibleconfigfile}.orig | less -r
```

### Ansible Comands


### Ansible Files
#### cfg
#### inventory
#### playbook




### Ansible Projects


<!--stackedit_data:
eyJoaXN0b3J5IjpbMzY0MzgxMDZdfQ==
-->