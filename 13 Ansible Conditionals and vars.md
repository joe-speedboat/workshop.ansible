# Ansible Conditionals and vars
Since Ansible is able to use facs and vars, we do a short review of how you can use that withing your playbooks and roles.

Here you can find a brief overview about conditions you can use out of the box
https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html

For that we use a project we created before:
```bash
cd /etc/ansible/projects/demo_role
```
* <code>install_install_upgrade_nginx.yml</code>
```yaml
- hosts: linux
  tasks:
  - name: install webserver
    yum: 
      name: lighttpd

  - lineinfile: path=/etc/lighttpd/lighttpd.conf regexp='^server.use-ipv6' line='server.use-ipv6 = "disable"'
  - copy:
      dest: /var/www/lighttpd/index.html
      content: |
        Managed by Ansible
        Hostname: {{ ansible_hostname }}
        IP: {{ ansible_default_ipv4.address|default(ansible_all_ipv4_addresses[0])}}
        Distribution: {{ ansible_distribution }}
        Distribution Version: {{ ansible_distribution_version }}
        

  - service: name=lighttpd enabled=yes state=started
  - service: name=firewalld enabled=yes state=started

  - firewalld: service=http permanent=true immediate=true state=enabled

```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY1NDc0MTY4MSwtMTY1MTUxODMzOCw5Nz
c3NzIwODBdfQ==
-->