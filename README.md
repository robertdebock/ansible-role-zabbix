zabbix
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-zabbix.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-zabbix)

Provides Zabbix for your system.

Context
-------
This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/zabbix.png "Dependency")

Requirements
------------

Access to a repository containing packages, likely on the internet.
Access to Extra Packages for Enterprise Linux. (Hint: robertdebock.epel)
Apache HTTPD for a Zabbix web interface. (Hint: robertdebock.httpd)

Role Variables
--------------

- zabbix_version_major: Part of the version of Zabbix.
- zabbix_version_minor: Part of the version of Zabbix.
- zabbix_version: Combined details to find the Zabbix version.
- zabbix_agent: Zabbix agent present or absent.
- zabbix_agent_server_address: IP address to listen on.
- zabbix_agent_listen_port: TCP port to allow connections on.
- zabbix_agent_server_active_address: IP address or hostname to get config from.
- zabbix_get: Zabbix get present or absent.
- zabbix_java_gateway: Zabbix java_gateway present or absent.
- zabbix_server: Zabbix server present or absent.
- zabbix_server_type: Type of database, mysql.
- zabbix_server_database_host: Database connection details.
- zabbix_server_database_name: Database connection details.
- zabbix_server_database_user: Database connection details.
- zabbix_server_database_pass: Database connection details.

Dependencies
------------

Soft dependencies that prepare your system for Zabbix:

- [robertdebock.bootstrap](https://travis-ci.org/robertdebock/ansible-role-bootstrap)
- [robertdebock.epel](https://travis-ci.org/robertdebock/ansible-role-epel)
- [robertdebock.httpd](https://travis-ci.org/robertdebock/ansible-role-httpd) (For a Zabbix web frontend.)

Download the dependencies by issuing this command:
```
ansible-galaxy install --role-file requirements.yml
```

Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.4|ansible 2.5|ansible 2.6|
|------------|-----------|-----------|-----------|
|alpine-edge|yes|yes|yes|
|alpine-latest|yes|yes|yes|
|archlinux|yes|yes|yes|
|centos-6|yes|yes|yes|
|centos-latest|yes|yes|yes|
|debian-latest|yes|yes|yes|
|debian-stable|yes|yes|yes|
|fedora-latest|no|no|no|
|fedora-rawhide|no|no|no|
|opensuse-leap|no|no|no|
|opensuse-tumbleweed|no|no|no|
|ubuntu-artful|no|no|no|
|ubuntu-latest|yes|yes|yes|

Example Playbook
----------------

To install an agent to all `servers`:
```
- hosts: servers
  become: yes

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.epel
    - role: robertdebock.zabbix
```

To install a Zabbix environment:
```
- hosts: databaseserver
  become: yes

  roles:
    - robertdebock.bootstrap
    - robertdebock.mysql

    tasks:
      - name: create database
        mysql_db:
          name: zabbix

      - name: create user
        mysql_user:
          name: zabbix
          password: zabbix
          priv: '*.*:ALL'

- hosts: zabbixserver
  become: yes

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.epel
    - role: robertdebock.zabbix
      zabbix_server: present
      zabbix_server_database_host: "{{ hostvars['databaseserver']['ansible_default_ipv4']['address'] }}"

- hosts: all
  become: yes

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.epel
    - role: robertdebock.
      zabbix_agent_server_address: ""{{ hostvars['zabbixserver']['ansible_default_ipv4']['address'] }}"


- hosts: zabbixwebserver

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.buildtools
    - role: robertdebock.epel
    - role: robertdebock.scl
    - role: robertdebock.httpd
    - role: robertdebock.php
    - role: robertdebock.zabbix
      zabbix_web: present
      zabbix_web_server: localhost
```

Install this role using `galaxy install robertdebock.zabbix`.

License
-------

Apache License, Version 2.0

Author Information
------------------

Robert de Bock](https://robertdebock.nl/) <robert@meinit.nl>
