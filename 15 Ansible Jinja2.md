# Ansible & Jinja2 
Jinja is not widely known, but really valuable if you pair it with ansible
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
        {{ vars.ansible_hostname }};{{ vars.remote_host }};{{ vars.ansible_fqdn }};{{ vars.ansible_distribution }};{{ vars.ansible_distribution_version }};{{ vars.ansible_processor[1] }};{{ vars.ansible_architecture }};{{ (vars.ansible_memtotal_mb/1024)|round|int }};  # NOQA
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

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NjAzNTQ1NzZdfQ==
-->