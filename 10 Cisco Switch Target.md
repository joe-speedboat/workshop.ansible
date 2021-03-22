
# Cisco Switch Targets
Since Ansible can configure lot of target types, we look at Cisco devices now.
The only tricky thing is to get a working example config, but lets create one.

### Create Ansible Project Files

#### Project
```bash
PNAME="Cisco_Switch"
PDIR="/etc/ansible/projects/cisco_switch"
mkdir -p $PDIR
chmod 700 $PDIR
```
#### Inventory
* <code>$PDIR/inventory</code>
```ini
# ansible demo inventory for $PNAME
[cisco_switch]
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
* <code>$PDIR/cisco_switch_backup.yml</code>
```yaml
---
- hosts: switch1
  gather_facts: no
  connection: local
  vars:
    backup_dir: /etc/ansible/projects/cisco_switch/backup
    creds:
      host: "{{ inventory_hostname }}"
      username: admin
      password: logINPass...
      auth_pass: enablePass...
      authorize: yes
  tasks:
  - name: save running config to device
    ios_config:
      save_when: always
      provider: "{{ creds }}"
  - name: get cisco switch config
    ios_command:
      commands: 
      - show running-config
      provider: "{{ creds }}"
    register: config
  - name: ensure backup folder is created
    file:
      path: "{{ backup_dir }}/"
      state: directory
    run_once: yes
  - name: get timestamp
    command: date +%Y%m%d
    register: timestamp
    run_once: yes
  - name: save output to {{ backup_root }} 
    copy: 
      content: "{{ config.stdout[0] }}"
      dest: "{{ backup_root }}/show_run_{{ inventory_hostname }}_{{ timestamp.stdout }}.txt"
  - name: get device config
    ios_command:
      commands:
      - show vlan brief
      - show interface status
      - show ip arp
      - show cdp neighbors
      - show version
      provider: "{{ creds }}"
    register: config
  - name: save output to {{ backup_root }}
    copy:
      content: |
        {{ config.stdout[0] }}
        ----------------------
        {{ config.stdout[1] }}
        ----------------------
        {{ config.stdout[2] }}
        ----------------------
        {{ config.stdout[3] }}
        ----------------------
        {{ config.stdout[4] }}
      dest: "{{ backup_root }}/doku_{{ inventory_hostname }}_{{ timestamp.stdout }}.txt"
...
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3OTExNTI3MjNdfQ==
-->