zabbix
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-zabbix.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-zabbix)

The purpose of this role is to install and configure Zabbix on your system.

Example Playbook
----------------

This example is taken from `molecule/default/playbook.yml`:
```
---
- name: Converge
  hosts: all
  gather_facts: false

  roles:
    - robertdebock.bootstrap
    - robertdebock.epel
    - robertdebock.zabbix

```

Role Variables
--------------

These variables are set in `defaults/main.yml`:
```
---
# defaults file for zabbix

# The "zabbix_version_major" are two numerical values, sparated by a period.
zabbix_version_major: 4.0

zabbix_version_minor: 1
# The "zabbix_version" is the whole version of Zabbix.
zabbix_version: "{{ zabbix_version_major }}.{{ zabbix_version_minor }}-1"

# Zabbix Agent settings.
# "zabbix_agent" can be "present", "absent" or unset. (unset defaults to "present")
zabbix_agent: present
# Values used in the zabbix_agentd.conf template.
zabbix_agent_server_address: 127.0.0.1
zabbix_agent_listen_port: 10050
zabbix_agent_server_active_address: 127.0.0.1

# "zabbix_get" can be "present", "absent" or unset. (unset defaults to "absent")
zabbix_get: absent

# "zabbix_java_gateway" can be "present", "absent" or unset. (unset defaults to "absent")
zabbix_java_gateway: absent

# "zabbix_server" can be "present", "absent" or unset. (unset defaults to "absent")
zabbix_server: absent
# "zabbix_server_type" can be "mysql" "pgsql" or unset. (unset defaults to "mysql")
zabbix_server_type: mysql
# The connection to MySQL or PostgreSQL must be specified.
zabbix_server_database_host: localhost
zabbix_server_database_name: zabbix
zabbix_server_database_user: zabbix
zabbix_server_database_pass: zabbix

# "zabbix_server" can be "present", "absent" or unset. (unset defaults to "absent")
zabbix_sender: absent

# "zabbix_proxy" can be "present", "absent" or unset. (unset defaults to "absent")
zabbix_proxy: absent
# "zabbix_proxy_type" can be "mysql" "pgsql" or unset. (unset defaults to "mysql")
zabbix_proxy_type: mysql

# "zabbix_proxy" can be "present", "absent" or unset. (unset defaults to "absent")
zabbix_web: absent
# "zabbix_web_type" can be "mysql" "pgsql" or unset. (unset defaults to "mysql")
zabbix_web_type: mysql
# The Zabbix server to connect to.
zabbix_web_server: localhost
zabbix_web_server_port: 10051
zabbix_web_server_name: zabbix

# To update all packages installed by this roles, set `zabbix_package_state` to `latest`.
zabbix_package_state: present

```

Requirements
------------

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the last 3 release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

---
- robertdebock.bootstrap
- robertdebock.epel


Context
-------

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/zabbix.png "Dependency")


Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.4|ansible 2.5|ansible 2.6|ansible 2.7|ansible devel|
|------------|-----------|-----------|-----------|-----------|-------------|
|alpine-edge*|yes|yes|yes|yes|yes*|
|alpine-latest|yes|yes|yes|yes|yes*|
|archlinux|yes|yes|yes|yes|yes*|
|centos-6|yes|yes|yes|yes|yes*|
|centos-latest|yes|yes|yes|yes|yes*|
|debian-latest|yes|yes|yes|yes|yes*|
|debian-stable|yes|yes|yes|yes|yes*|
|debian-unstable*|yes|yes|yes|yes|yes*|
|fedora-latest|no|no|no|no|no*|
|fedora-rawhide*|no|no|no|no|no*|
|opensuse-leap|no|no|no|no|no*|
|opensuse-tumbleweed|no|no|no|no|no*|
|ubuntu-artful|no|no|no|no|no*|
|ubuntu-devel*|yes|yes|yes|yes|yes*|
|ubuntu-latest|yes|yes|yes|yes|yes*|

A single star means the build may fail, it's marked as an experimental build.

Testing
-------

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-zabbix) are done on every commit and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-zabbix/issues)

To test this role locally please use [Molecule](https://github.com/metacloud/molecule):
```
pip install molecule
molecule test
```
There are many specific scenarios available, please have a look in the `molecule/` directory.


License
-------

Apache-2.0


Author Information
------------------

[Robert de Bock](https://robertdebock.nl/) <robert@meinit.nl>
