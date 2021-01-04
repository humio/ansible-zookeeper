ansible-zookeeper
=================

ZooKeeper playbook for Ansible

Installation
-----------

```bash
ansible-galaxy install humio.zookeeper
```

Dependencies
------------

Java

 - https://github.com/humio/ansible-java

Requirements
------------

Ansible version at least 2.4

Role Variables
--------------

See the [defaults](https://github.com/humio/ansible-zookeeper/blob/master/defaults/main.yml)

Example Playbook
----------------

```yaml
- name: Installing ZooKeeper
  hosts: all
  sudo: yes
  roles:
    - role: humio.zookeeper
```

Cluster Example
----------------

```yaml
- name: Zookeeper cluster setup
  hosts: zookeepers
  sudo: yes
  roles:
    - role: humio.zookeeper
      zookeeper_hosts: "{{groups['zookeepers']}}"
```

Assuming ```zookeepers``` is a [hosts group](http://docs.ansible.com/ansible/intro_inventory.html#group-variables) defined in inventory file.

```inventory
[zookeepers]
server[1:3]
```

Custom IP per host group

```
zookeeper_hosts: "
    {%- set ips = [] %}
    {%- for host in groups['zookeepers'] %}
    {{- ips.append(dict(id=loop.index, host=host, ip=hostvars[host]['ansible_default_ipv4'].address)) }}
    {%- endfor %}
    {{- ips -}}"
```

License
-------

The MIT License (MIT)

Copyright (c) 2014 Kien Pham

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
