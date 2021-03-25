# Ansible & Jinja2 
Jinja is not widely known, but really valuable if you pair it with ansible, it gets even better!

https://jinja.palletsprojects.com/en/2.11.x/templates/
https://docs.ansible.com/ansible/latest/user_guide/playbooks_templating.html


## Jinja2 in Playbooks
### Let's just create one
```bash
cd /etc/ansible/projects/demo_role
```
* <code>create_inventory_with_jinja2.yml</code>
```yaml
- hosts: all
  tasks:
  - copy:
      content: |
        HOSTNAME;IPADDRESS;FQDN;OSNAME;OSVERSION;PROCESSOR;ARCHITECTURE;MEMORY;
        {% for host in hostvars %}
        {%   set vars = hostvars[host|string] %}
        {% if vars.ansible_system == "Win32NT" %}
        {{ vars.ansible_hostname }};{{ vars.ansible_ip_addresses[0] }};{{ vars.ansible_fqdn }};{{ vars.ansible_distribution }};{{ vars.ansible_distribution_version }};{{ vars.ansible_processor[1] }};{{ vars.ansible_architecture }};{{ (vars.ansible_memtotal_mb/1024)|round|int }};  
        {% elif vars.ansible_system == "Linux" %}
        {{ vars.ansible_hostname }};{{ vars.ansible_default_ipv4.address }};{{ vars.ansible_fqdn }};{{ vars.ansible_distribution }};{{ vars.ansible_distribution_version }};{{ vars.ansible_processor[1] }};{{ vars.ansible_architecture }};{{ (vars.ansible_memtotal_mb/1024)|round|int }};  
        {% endif %}
        {% endfor %}
      dest: systems.csv
      backup: yes
    run_once: yes
    delegate_to: localhost
...

```
* call the playbook
* examine the results
* review the playbook and try to understand what happened
* explain what happened

## Jinja2 in Templates
### Let's just create one
```bash
cd /etc/ansible/projects/demo_role
mkdir
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1ODA2ODgzODgsNjI5ODMwNzI4XX0=
-->