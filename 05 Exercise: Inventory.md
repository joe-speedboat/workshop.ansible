


# Ansible Inventory Exercise
Okay, enough theory for now, lets check if we understood how inventories, configs and projects fit together.

# create new project
* In directory: <code>/etc/my_projects/myinventory</code>
* Create an "ansible config" file for this project
* Create an inventory file with the name: <code>infra</code>
* Create Host group <code>win</code> with two hosts <code>winX</code> (X is the number)
* Create Host group <code>win</code> with two hosts <code>dcX</code> (X is the number)
* Create host group called <code>dns</code> with two hosts <code>dcX</code> (X is the number)
* Create host group <code>lin</code>
	* With two hosts <code>linX</code> (X is the number)
* The dcX and linX must have the var <code>ntp_srv -> ch.pool.ntp.org</code>
* The winX host must not have the var <code>ntp_srv</code> assigned

* Think before declaring about what you want to declare!
* Ask if something is unclear
* Use commands you have seen before to verify then inventory content
* Declare the inventory as minimal and lean as possible

## Solution
If all of you have finished the lab, do a review in class and explain each other what and why you did to solve the exercise.


### Hints
* Perms
* Group Vars
* Nested Groups

##Background
Variables can be defined on multiple places and multiple times, try to avoid that.
To not end up in a mess, test carefully which is the best place for an information, before defining it.
Here you can find how variable definitions get voted/handled:
https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable

In short, this is it:




1.  command line values (for example, `-u my_user`, these are not variables)
2.  role defaults (defined in role/defaults/main.yml) [1](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id13)
3.  inventory file or script group vars [2](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id14)
4.  inventory group_vars/all [3](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id15)
5.  playbook group_vars/all [3](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id15)
6.  inventory group_vars/* [3](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id15)
7.  playbook group_vars/* [3](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id15)
8.  inventory file or script host vars [2](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id14)
9.  inventory host_vars/* [3](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id15)
10.  playbook host_vars/* [3](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id15)
11.  host facts / cached set_facts [4](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#id16)
12.  play vars
13.  play vars_prompt
14.  play vars_files
15.  role vars (defined in role/vars/main.yml)
16.  block vars (only for tasks in block)
17.  task vars (only for the task)
18.  include_vars
19.  set_facts / registered vars
20.  role (and include_role) params
21.  include params
22.  extra vars (for example, `-e "user=my_user"`)(always win precedence


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MzE1OTU2MDksLTIzNDAwMzEwOV19
-->