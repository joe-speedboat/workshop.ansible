


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
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM5MjU3Nzc1MV19
-->