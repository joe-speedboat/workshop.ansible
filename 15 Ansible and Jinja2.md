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
### OK, we just create one
```bash
cd /etc/ansible/projects/demo_role/roles
ansible-galaxy init demo.jinja2
cd /etc/ansible/projects/demo_role
```
Please create and try to understand this files carefully:

* <code>roles/demo.jinja2/defaults/main.yml</code>
```yaml
---
# defaults file for demo.jinja2
# this vars are only mentioned for understanding, please define your own config outside of the role
lb_backend:
- name: fail_xxx
  lbport: 8080
  lbback:
  - name: web1
    host: 127.0.0.1
    port: 80
  - name: web2
    host: 1.2.3.5
    port: 80
- name: http2
  lbport: 8081
  lbback:
  - name: web3
    host: 2.2.3.4
    port: 80
  - name: web4
    host: 2.2.3.5
    port: 80

# admin ui
stats_port: 10000
stats_usr: admin
stats_pw: redhat
...
```

* <code>roles/demo.jinja2/handlers/main.yml</code>
```yaml
---
# handlers file for demo.jinja2
- name: restart haproxy
  service:
    name: haproxy
    state: restarted
...
```

* <code>roles/demo.jinja2/meta/main.yml</code>
```yaml
---
galaxy_info:
  author: chris@bitbull.ch
  description: HA-Proxy demo role for teaching only
  company: uniQconsulting ag
  license: license (GPL-2.0-or-later, MIT, etc)
  min_ansible_version: 2.9
  platforms:
  - name: Centos
    versions:
    - 8
  galaxy_tags:
  - haproxy
  - teaching
dependencies: []
...
```
* <code>roles/demo.jinja2/tasks/main.yml</code>
```yaml
---
# tasks file for demo.jinja2
- name: avoid using of default variables
  fail:
    msg:
      please look into defaults/main.yml, loadbalancer config cannot be a guess
  when: item.name == "fail_xxx"
  with_items: 
  - "{{ lb_backend }}"

- name: open firewall ports
  firewalld:
    port: "{{ item.lbport }}/tcp"
    permanent: true 
    immediate: true 
    state: enabled
  with_items:
  - "{{ lb_backend }}"

- name: open firewall ports
  firewalld:
    port: "{{ stats_port }}/tcp"
    permanent: true 
    immediate: true 
    state: enabled

- name: install haproxy
  dnf:
    name: "{{ packages }}"
  notify:
  - restart haproxy

- name: create haproxy config
  template: 
    src: haproxy.cfg.j2 
    dest: /etc/haproxy/haproxy.cfg 
    owner: haproxy 
    group: root 
    mode: 0660
  notify:
  - restart haproxy

- name: enable services
  service:
    name: "{{ item }}"
    enabled: yes
    state: started
  with_items: "{{ services }}"
...
```
* <code>roles/demo.jinja2/templates/haproxy.cfg.j2</code>
```jinja2
global
  log /dev/log  local0
  log /dev/log  local1 notice
  stats socket /var/run/haproxy.sock mode 0600 level admin
  chroot /var/lib/haproxy
  user haproxy
  group haproxy
  daemon

defaults
  log global
  mode  http
  option  httplog
  option  dontlognull
        timeout connect 5000
        timeout client 50000
        timeout server  5000

{% for svc in lb_backend %}
frontend {{ svc.name }}
    bind *:{{ svc.lbport }}
    mode http
    default_backend {{ svc.name }}_backend

backend {{ svc.name }}_backend
    mode http
    balance roundrobin
    option forwardfor
    option httpchk HEAD / HTTP/1.1\r\nHost:localhost
    cookie SERVERID insert indirect
    {% for srv in svc.lbback %}
    server {{ srv.name }} {{ srv.host }}:{{ srv.port }} cookie {{ srv.name }} check inter 2000
    {% endfor %}

{% endfor %}

listen stats *:{{ stats_port }}
    stats enable
    stats uri /
    stats hide-version
    stats auth {{ stats_usr }}:{{ stats_pw }}
```
* <code>roles/demo.jinja2/tests/test.yml</code>
```yaml
---
- hosts: localhost
  remote_user: root
  vars:
    lb_backend:
    - name: http1
      lbport: 8080
      lbback:
      - name: web1
        host: 127.0.0.1
        port: 80
      - name: web2
        host: 1.2.3.5
        port: 80
    - name: http2
      lbport: 8081
      lbback:
      - name: web3
        host: 2.2.3.4
        port: 80
      - name: web4
        host: 2.2.3.5
        port: 80

    stats_port: 10000
    stats_usr: admin
    stats_pw: ansible

  roles:
    - demo.jinja2
...
```
* <code>roles/demo.jinja2/vars/main.yml</code>
```yaml
---
# vars file for demo.jinja2
packages:
- haproxy
- firewalld

services:
- haproxy
- firewalld
...
```
* Step back, do you understand what happened in this role?
* Explain it to me!

### Let's kick the tires and light the fires
```bash
cp roles/demo.jinja2/tests/test.yml haproxy.yml
vi




<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNTAzNjk2MTksLTE1NTA2MjA3NTAsLT
EyOTYxNTc0MTksNjI5ODMwNzI4XX0=
-->