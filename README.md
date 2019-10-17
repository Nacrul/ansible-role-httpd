httpd
=========

<img src="https://docs.ansible.com/ansible-tower/3.2.4/html_ja/installandreference/_static/images/logo_invert.png" width="10%" height="10%" alt="Ansible logo" align="right"/>
<a href="https://travis-ci.org/robertdebock/ansible-role-httpd"><img src="https://travis-ci.org/robertdebock/ansible-role-httpd.svg?branch=master" alt="Build status" align="left"/></a>

Install and configure httpd on your system.

<img src="https://img.shields.io/ansible/role/d/21855"/>
<img src="https://img.shields.io/ansible/quality/21855"/>

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
    httpd_locations:
      - name: mylocation1
        location: /mylocation1
        backend_url: http://localhost:8080/myapplication
    httpd_vhosts:
      - name: docroot
        servername: www1.example.com
        documentroot: /var/www/html/www1.example.com
      - name: backend_http
        servername: www2.example.com
        backend_url: http://www.example.com/
      - name: remote
        servername: www3.example.com
        remote: http://localhost:3128/
      - name: backend_https
        servername: www4.example.com
        backend_url: https://www.example.com/

  roles:
    - robertdebock.httpd
```

The machine you are running this on, may need to be prepared.
```yaml
---
- name: Prepare
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - robertdebock.bootstrap
    - robertdebock.epel
    - robertdebock.buildtools
    - robertdebock.python_pip
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

Role Variables
--------------

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for httpd

# The servername to use.
httpd_servername: "{{ ansible_fqdn }}"

# The non-SSL port to use.
httpd_port: 80

# To configure https, set the hostname to listen to.
httpd_ssl_servername: "{{ ansible_fqdn }}"

# For SSL a TCP port is required.
httpd_ssl_port: 443
```

Requirements
------------

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.buildtools
- robertdebock.epel
- robertdebock.python_pip

```

This role uses the following modules:
```yaml
---
- command
- file
- openssl_certificate
- openssl_csr
- openssl_privatekey
- package
- pip
- seboolean
- seport
- service
- template
```

Context
-------

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/httpd.png "Dependency")


Compatibility
-------------

This role has been tested on these [container images](https://hub.docker.com/):

|container|allow_failures|
|---------|--------------|
|alpine:latest|no|
|alpine:edge|yes|
|archlinux/base|no|
|robertdebock/docker-centos-systemd:7|no|
|robertdebock/docker-centos-systemd:latest|no|
|robertdebock/docker-debian-systemd:latest|no|
|robertdebock/docker-debian-systemd:stable|no|
|robertdebock/docker-debian-systemd:unstable|yes|
|robertdebock/docker-fedora-systemd:latest|no|
|robertdebock/docker-fedora-systemd:rawhide|yes|
|opensuse/leap|no|
|robertdebock/docker-ubuntu-systemd:latest|no|
|robertdebock/docker-ubuntu-systemd:devel|yes|
|robertdebock/docker-ubuntu-systemd:rolling|no|

This role has been tested on these Ansible versions:

- ansible~=2.7
- ansible~=2.8
- git+https://github.com/ansible/ansible.git@devel

The indicator '~=' means [compatible with](https://www.python.org/dev/peps/pep-0440/#compatible-release). For example 'ansible~=2.8' would pick the latest ansible-2.8, for example ansible-2.8.5.




Testing
-------

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-httpd) are done on every commit and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-httpd/issues)

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
