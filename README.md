zabbix
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-zabbix.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-zabbix)

Provides Zabbix for your system.

Context
--------
This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/robertdebock.github.io/artifacts/zabbix.png "Dependency")

Requirements
------------

Access to a repository containing packages, likely on the internet.
Access to Extra Packages for Enterprise Linux. (Hint: robertdebock.epel)
Apache HTTPD for a Zabbix web interface. (Hint: robertdebock.httpd)

Role Variables
--------------

None known

Dependencies
------------

Soft dependencies that prepare your system for Zabbix:

- robertdebock.bootstrap
- robertdebock.epel
- robertdebock.httpd (For a Zabbix web frontend.)

Download the dependencies by issuing this command:
```
ansible-galaxy install --role-file requirements.yml
```

Example Playbook
----------------

```
- hosts: servers

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.epel
    - role: robertdebock.zabbix
```

Install this role using `galaxy install robertdebock.zabbix`.

License
-------

Apache License, Version 2.0

Author Information
------------------

Robert de Bock <robert@meinit.nl>
