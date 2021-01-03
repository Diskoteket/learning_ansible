# Ad-hoc commands
## Check if ansible can reach all hosts in inventory
```bash
ansible all --key-file ~/.ssh/ansible -i inventory -m ping
```
If you have created an inventory file:
```bash
ansible all -m ping
```

## Gather facts about machines in inventory
```bash
ansible all -m gather_facts
```
If you want to limit your facts:
```bash
ansible all -m gather_facts --limit 192.168.122.101
```

## Elevated ad-hoc commands
Assuming you have the same password for all servers:
```bash
ansible all -m apt -a update_cache=true --become --ask-become-pass
```
# Playbook stuff
## Using "When"
```yml
 - name: do stuff when a specific distro and version is present
   apt:
     name: apache2
     state: latest
   when: ansible_distribution == "Ubuntu" and ansible_distribution_major_version == "20"
```

```yml
 - name: do stuff when there are a range of distros but they use the same package manager
   apt:
     update_cache: yes
   when: ansible_distribution in ["Debian", "Ubuntu"]
```
## Variables first example
```yml
# install_apache.yml

  - name: install apache2 and php packages
    package:
      name: 
        - "{{ apache_package }}"
        - "{{ php_package }}"
      state: latest
      update_cache: yes
```

```bash
# inventory file

# Ubuntu servers
192.168.122.101 apache_package=apache2 php_package=libapache2-mod-php
192.168.122.102 apache_package=apache2 php_package=libapache2-mod-php
192.168.122.103 apache_package=apache2 php_package=libapache2-mod-php

# RHEL server
192.168.122.104 apache_package=httpd php_package=php
```

## Inventory groups
``` bash
# inventory file

[web_servers]
192.168.122.101
192.168.122.104

[file_servers]
192.168.122.102

[db_servers]
192.168.122.10
```

```yml
# provision_stuff.yml

- hosts: db_servers
  become: true
  tasks:

  - name: install mariadb package (RHEL/CentOS)
    dnf:
      name: mariadb
      state: latest
    when: ansible_distribution in ["RedHat", "CentOS"]

  - name: install mariadb package (Debian/Ubuntu)
    apt:
      name: mariadb-server
      state: latest
    when: ansible_distribution in ["Debian", "Ubuntu"]
```

## Tags
``` bash
# listing the tags of a playbook

ansible-playbook --list-tags prov_servers.yml
```

``` bash
# run a playbook, targeting a single tag

ansible-playbook --tags centos --ask-become-pass prov_servers.yml
```

``` bash
# run a playbook, targeting multiple tags

ansible-playbook --tags "apache,db" --ask-become-pass prov_servers.yml
```
