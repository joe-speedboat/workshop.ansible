


# Ansible Inventory Exercise
Okay, enough theory for now, lets check if we understood how inventories, configs and projects fit together.

# create new project
* In directory: <code>/etc/my_projects/myinventory</code>
* Create an "ansible config" file for this project
* Create an inventory file with the name: <code>infra</code>
* Create Host group <code>win</code>
	* With two hosts <code>winX</code> (X is the number)
	* With two hosts <code>dcX</code> (X is the number)
* Create host group called </code>dns</code> with two hosts <code>dcX</code> (X is the number)
* Create host group <code>lin</code>
	* With two hosts <code>linX</code> (X is the number)
* The dcX and linX must have the var <code>ntp_srv -> ch.pool.ntp.org</code>
* The winX host must not have the var <code>ntp_srv</code> assigned
* Use the a
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MDcwOTQxNTRdfQ==
-->