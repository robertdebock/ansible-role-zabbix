zabbix
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-zabbix.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-zabbix)

Install and configure Zabbix on your system.

Example Playbook
----------------

This example is taken from `molecule/resources/playbook.yml`:
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  vars:
    zabbix_server: present
    zabbix_web: present

  roles:
    - robertdebock.httpd
    - robertdebock.mysql
    - robertdebock.zabbix
```

The machine you are running this on, may need to be prepared.
```yaml
---
- name: Prepare
  hosts: all
  become: yes
  gather_facts: no

  vars:
    mysql_databases:
      - name: zabbix
        encoding: utf8
        collation: utf8_bin
    mysql_users:
      - name: zabbix
        password: zabbix
        priv: "zabbix.*:ALL"

  roles:
    - robertdebock.bootstrap
    - robertdebock.epel
    - robertdebock.python_pip
    - robertdebock.php
    - robertdebock.mysql
    - robertdebock.httpd
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

Role Variables
--------------

These variables are set in `defaults/main.yml`:
```yaml
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
zabbix_server_database_port: 3306
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
zabbix_web_server: "https://localhost/zabbix"
zabbix_web_server_port: 10051
zabbix_web_server_name: zabbix

zabbix_web_username: Admin
zabbix_web_password: zabbix
zabbix_validate_certs: no

# You can provision Zabbix groups.
# Most options map directly to the documentation:
# https://docs.ansible.com/ansible/latest/modules/zabbix_group_module.html
zabbix_web_groups:
  - name: Linux servers

# Add hosts to Zabbix.
# Most options map directly to the documentation:
# https://docs.ansible.com/ansible/latest/modules/zabbix_host_module.html
zabbix_web_hosts:
  - name: Example server 1
    interface_ip: 192.168.127.127
    interface_dns: server1.example.com
    visible_name: Example server 1 name
    description: Example server 1 description
    groups:
      - Linux servers
    link_templates:
      - Template OS Linux
```

Requirements
------------

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the last 3 release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.epel
- robertdebock.mysql
- robertdebock.php
- robertdebock.python_pip
- robertdebock.httpd

```

Context
-------

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/zabbix.png "Dependency")


Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.6|ansible 2.7|ansible devel|
|------------|-----------|-----------|-------------|
|alpine-edge*|no|no|no*|
|alpine-latest|no|no|no*|
|archlinux|no|no|no*|
|centos-6|no|no|no*|
|centos-latest|no|yes|yes*|
|debian-latest|no|yes|yes*|
|debian-stable|no|yes|yes*|
|debian-unstable*|no|yes|yes*|
|fedora-latest|no|no|no*|
|fedora-rawhide*|no|no|no*|
|opensuse-leap|no|no|no*|
|ubuntu-devel*|no|yes|yes*|
|ubuntu-latest|no|yes|yes*|
|ubuntu-rolling|no|no|no*|

A single star means the build may fail, it's marked as an experimental build.

Testing
-------

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-zabbix) are done on every commit and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-zabbix/issues)

To test this role locally please use [Molecule](https://github.com/ansible/molecule):
```
pip install molecule
molecule test
```

To test on Amazon EC2, configure [~/.aws/credentials](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html) and `export AWS_REGION=eu-central-1` before running `molecule test --scenario-name ec2`.

There are many specific scenarios available, please have a look in the `molecule/` directory.

Run the [ansible-galaxy](https://github.com/ansible/galaxy-lint-rules) and [my](https://github.com/robertdebock/ansible-lint-rules) lint rules if you want your change to be merges:

```shell
git clone https://github.com/ansible/ansible-lint.git /tmp/ansible-lint
ansible-lint -r /tmp/ansible-lint/lib/ansiblelint/rules .

git clone https://github.com/robertdebock/ansible-lint /tmp/my-ansible-lint
ansible-lint -r /tmp/my-ansible-lint/rules .
```

License
-------

Apache-2.0


Author Information
------------------

Robert de Bock
