## Troubleshoot Ansible

Just create and try to fix the playbook and run it

```bash
cd /etc/ansible/projects/demo_role
```
 * <code>debug.yml</code>
```yaml
- hosts: vm11
  tasks:
- name: install webserver
    dnf: 
      name: 
      - lighttpd
      - firewalld
      state: latest
    register: install

- lineinfile: 
     path=/etc/lighttpd/lighttpd.conf 
     regexp='^server.use-ipv6' 
     line='server.use-ipv6 = "disable"'
 - copy:
      dest: /var/www/lighttpd/index.html
      content: |
        Hostname: "{{ansible_hostname}}"
        
- name: enable and start services
    service:
      name: "{{ item }}"
      enabled: yes
      state: started
    with_items:
    - firewalld
    - lighttpd

 - name: restart services if needed by software installation
    service:
      name: "{{ item }}"
      enabled: yes
      state: started
    with_items:
    - firewalld
    - lighttpd
    when: install.changed

 - name: open firewall port 80
    firewalld: 
      service: http 
      permanent: true 
      immediate: true 
      state: enabled
...
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDU1MDYyMDYwXX0=
-->