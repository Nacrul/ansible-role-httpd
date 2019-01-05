httpd
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-httpd.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-httpd)

Install and configure httpd on your system.

Example Playbook
----------------

This example is taken from `molecule/default/playbook.yml`:
```yaml
---
- name: Converge
  hosts: all
  gather_facts: no
  become: yes

  vars:
    httpd_locations:
      - name: mylocation1
        location: /mylocation1
        backend_url: http://localhost:8080/myapplication
    httpd_vhosts:
      - name: myvhost1
        servername: www1.example.com
        documentroot: /var/www/html/www1.example.com
      - name: myvhost2
        servername: www2.example.com
        documentroot: /var/www/html/www2.example.com
        backend_url: http://www.example.com/
      - name: myvhost3
        servername: www3.example.com
        documentroot: /var/www/html/www3.example.com
        backend_url: http://www.example.com/
        remote: http://localhost:3128/

  roles:
    - robertdebock.bootstrap
    - robertdebock.epel
    - robertdebock.buildtools
    - robertdebock.python_pip
    - robertdebock.httpd

```

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

# To update all packages installed by this roles, set `httpd_package_state` to `latest`.
httpd_package_state: present

# Some Docker containers do not allow managing services, rebooting and writing
# to some locations in /etc. The role skips tasks that will typically fail in
# Docker. With this parameter you can tell the role to -not- skip these tasks.
httpd_ignore_docker: yes

```

Requirements
------------

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the last 3 release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.buildtools
- robertdebock.epel
- robertdebock.python_pip

```

Context
-------

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/httpd.png "Dependency")


Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.6|ansible 2.7|ansible devel|
|------------|-----------|-----------|-------------|
|alpine-edge*|yes|yes|yes*|
|alpine-latest|yes|yes|yes*|
|archlinux|yes|yes|yes*|
|centos-6|no|no|no*|
|centos-latest|yes|yes|yes*|
|debian-latest|yes|yes|yes*|
|debian-stable|yes|yes|yes*|
|debian-unstable*|yes|yes|yes*|
|fedora-latest|yes|yes|yes*|
|fedora-rawhide*|yes|yes|yes*|
|opensuse-leap|yes|yes|yes*|
|opensuse-tumbleweed|yes|yes|yes*|
|ubuntu-artful|yes|yes|yes*|
|ubuntu-devel*|yes|yes|yes*|
|ubuntu-latest|yes|yes|yes*|

A single star means the build may fail, it's marked as an experimental build.

Testing
-------

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-httpd) are done on every commit and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-httpd/issues)

To test this role locally please use [Molecule](https://github.com/metacloud/molecule):
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

[Robert de Bock](https://robertdebock.nl/) <robert@meinit.nl>
