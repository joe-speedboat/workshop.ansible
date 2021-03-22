# vCenter Targets
Note that the examples we looked ad before are working for vCenter targets as well.

If you fire against vCenter, some VARs not used in ESXi targets become useful:
* datacenter
* folder

So lets look what we can do with VMware Targets:

    ansible-doc -l | grep vmware

Lets Create an example Playbook that creates a linked clone of a VM:

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE2MzE5MTY0MF19
-->