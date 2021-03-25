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
        Managed by Ansible
        Hostname: {{ ansible_hostname }}
        IP: {{ ansible_default_ipv4.address|default(ansible_all_ipv4_addresses[0])}}
        Distribution: {{ ansible_distribution }}
        Distribution Version: {{ ansible_distribution_version }}
        CPU: {{ ansible_processor[1] }}
        ARCH: {{ ansible_architecture }}
        MEM: {{ (vars.ansible_memtotal_mb/1024)|round|int }} GB

 * name: enable and start services
    service:
      name: "{{ item }}"
      enabled: yes
      state: started
    with_items:
    - firewalld
    - lighttpd

 * name: restart services if needed by software installation
    service:
      name: "{{ item }}"
      enabled: yes
      state: started
    with_items:
    - firewalld
    - lighttpd
    when: install.changed

 * name: open firewall port 80
    firewalld: 
      service: http 
      permanent: true 
      immediate: true 
      state: enabled
...
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NTAzMjExNjhdfQ==
-->