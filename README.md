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

```yaml
---
zookeeper_version: 3.4.12
zookeeper_url: http://archive.apache.org/dist/zookeeper/zookeeper-{{zookeeper_version}}/zookeeper-{{zookeeper_version}}.tar.gz

client_port: 2181
init_limit: 5
sync_limit: 2
tick_time: 2000
zookeeper_autopurge_purgeInterval: 0
zookeeper_autopurge_snapRetainCount: 10
zookeeper_cluster_ports: "2888:3888"
zookeeper_max_client_connections: 60

zookeeper_data_dir: /var/zookeeper
zookeeper_log_dir: /var/log/zookeeper
zookeeper_dir: /opt/zookeeper-{{zookeeper_version}}
zookeeper_conf_dir: "{{zookeeper_dir}}/conf"
zookeeper_tarball_dir: /opt/src

zookeeper_hosts_hostname: "{{inventory_hostname}}"
# List of dict (i.e. {zookeeper_hosts:[{host:,id:},{host:,id:},...]})
zookeeper_hosts:
  - host: "{{zookeeper_hosts_hostname}}" # the machine running
    id: 1
```

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
