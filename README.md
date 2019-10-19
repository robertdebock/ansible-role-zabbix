zabbix
=========

<img src="https://docs.ansible.com/ansible-tower/3.2.4/html_ja/installandreference/_static/images/logo_invert.png" width="10%" height="10%" alt="Ansible logo" align="right"/>
<a href="https://travis-ci.org/robertdebock/ansible-role-zabbix"><img src="https://travis-ci.org/robertdebock/ansible-role-zabbix.svg?branch=master" alt="Build status" align="left"/></a>

Install and configure Zabbix on your system.

<img src="https://img.shields.io/ansible/role/d/23321"/>
<img src="https://img.shields.io/ansible/quality/23321"/>

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


  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.epel
    - role: robertdebock.python_pip
    - role: robertdebock.remi
      remi_enabled_repositories:
        - php73
    - role: robertdebock.php
    - role: robertdebock.mysql
      mysql_databases:
        - name: zabbix
          encoding: utf8
          collation: utf8_bin
      mysql_users:
        - name: zabbix
          password: zabbix
          priv: "zabbix.*:ALL"
    - role: robertdebock.httpd
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
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.epel
- robertdebock.mysql
- robertdebock.php
- robertdebock.python_pip
- robertdebock.httpd
- robertdebock.remi

```

This role uses the following modules:
```yaml
---
- apt
- command
- copy
- file
- meta
- mysql_db
- package
- pip
- seboolean
- service
- template
- zabbix_group
- zabbix_host
```

Context
-------

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/zabbix.png "Dependency")


Compatibility
-------------

This role has been tested on these [container images](https://hub.docker.com/):

|container|allow_failures|
|---------|--------------|
|docker-centos-systemd|no|
|docker-ubuntu-systemd|yes|

This role has been tested on these Ansible versions:

- ansible~=2.7
- ansible~=2.8
- git+https://github.com/ansible/ansible.git@devel

The indicator '~=' means [compatible with](https://www.python.org/dev/peps/pep-0440/#compatible-release). For example 'ansible~=2.8' would pick the latest ansible-2.8, for example ansible-2.8.5.

Exceptions
----------

Some variarations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| Alpine | Zabbix has [limited OS support](https://www.zabbix.com/download). |
| Archlinux | Zabbix has [limited OS support](https://www.zabbix.com/download). |
| CentOS 8 | Zabbix has [limited OS support](https://www.zabbix.com/download). |
| Debian | Zabbix has [limited OS support](https://www.zabbix.com/download). |
| Fedora | Zabbix has [limited OS support](https://www.zabbix.com/download). |
| openSUSE | Zabbix has [limited OS support](https://www.zabbix.com/download). |
| Ubuntu rolling | Zabbix has [limited OS support](https://www.zabbix.com/download). |

Included version(s)
-------------------

This role [refers to a version](https://github.com/robertdebock/ansible-role-zabbix/blob/master/defaults/main.yml) released by Zabbix. Check the released version(s) here:
- [Zabbix](https://www.zabbix.com/download).

This version reference means a role may get outdated. Monthly tests occur to see if [bit-rot](https://en.wikipedia.org/wiki/Software_rot) occured. If you however find a problem, please create an issue, I'll get on it as soon as possible.

Testing
-------

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-zabbix) are done on every commit and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-zabbix/issues)

To test this role locally please use [Molecule](https://github.com/ansible/molecule):
```
pip install molecule
molecule test
```

To test on Amazon EC2, configure [~/.aws/credentials](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html) and set a region using `export AWS_REGION=eu-central-1` before running `molecule test --scenario-name ec2`.

There are many specific scenarios available, please have a look in the `molecule/` directory.

License
-------

Apache-2.0


Author Information
------------------

[Robert de Bock](https://robertdebock.nl/)
