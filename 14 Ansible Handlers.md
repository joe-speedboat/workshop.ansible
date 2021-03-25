# Ansible Handlers
The exercise we did before is nice, but if you use Ansible on a daily base, I recommend to really learn all the possibilities you have to do the work.
So lets create a copy of the previous exercise and do something new with it:

```bash
cd /etc/ansible/projects/demo_role
cp install_upgrade_web.yml install_upgrade_web_handler.yml
```

Now we change the playbook slightly
* <code>install_upgrade_web_handler.yml</code>
```yaml
- hosts: linux
  tasks:
  - name: install webserver
    dnf: 
      name:
      - lighttpd
      - firewalld
      state: latest
    notify:
    - restart services

  - lineinfile: path=/etc/lighttpd/lighttpd.conf regexp='^server.use-ipv6' line='server.use-ipv6 = "disable"'
  - copy:
      dest: /var/www/lighttpd/index.html
      content: |
        Managed by Ansible
        Hostname: {{ ansible_hostname }}
        IP: {{ ansible_default_ipv4.address|default(ansible_all_ipv4_addresses[0])}}
        Distribution: {{ ansible_distribution }}
        Distribution Version: {{ ansible_distribution_version }}
        CPU: {{ ansible_processor[1] }}
        ARCH: {{ ansible_architecture }}
        MEM: {{ (vars.ansible_memtotal_mb/1024)|round|int }} GB

  - name: enable and start services
    service:
      name: "{{ item }}"
      enabled: yes
      state: started
    with_items:
    - firewalld
    - lighttpd

  - name: open firewall port 80
    firewalld: 
      service: http 
      permanent: true 
      immediate: true 
      state: enabled

  handlers:
  - name: restart services
    service:
      name: "{{ item }}"
      state: restarted
    with_items:
    - lighttpd
    - firewalld
...
```
* Run the playbook, no hander should be fired since we changed nothing
* So let's change somehting

    ansible -m dnf -a "state=absent name=lighttpd" linux
* Run the playbook again
* Can you see, the handlers got fired!?

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MzczNDgyMzcsMTk1OTM3NDMzOF19
-->