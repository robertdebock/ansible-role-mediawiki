# mediawiki

Install and configure mediawiki on your system.

|Travis|GitHub|Quality|Downloads|
|------|------|-------|---------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-mediawiki.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-mediawiki)|[![github](https://github.com/robertdebock/ansible-role-mediawiki/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-mediawiki/actions)|[![quality](https://img.shields.io/ansible/quality/29572)](https://galaxy.ansible.com/robertdebock/mediawiki)|[![downloads](https://img.shields.io/ansible/role/d/29572)](https://galaxy.ansible.com/robertdebock/mediawiki)|

## Example Playbook

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.mediawiki
      mediawiki_destination: /opt
```

The machine may need to be prepared using `molecule/resources/prepare.yml`:
```yaml
---
- name: Prepare
  hosts: all
  gather_facts: no
  become: yes

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.core_dependencies
    - role: robertdebock.epel
    - role: robertdebock.python_pip
    - role: robertdebock.buildtools
    - role: robertdebock.httpd
    - role: robertdebock.php
    - role: robertdebock.mysql
      mysql_databases:
        - name: mediawiki
      mysql_users:
        - name: mediawiki
          password: m3d14w1k1
          priv: "mediawiki.*:ALL"
```

For verification `molecule/resources/verify.yml` run after the role has been applied.
```yaml
---
- name: Verify
  hosts: all
  become: yes
  gather_facts: yes

  tasks:
    - name: connect to mediawiki
      get_url:
        url: https://localhost/mediawiki
        dest: /tmp/whatever
        validate_certs: no
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## Role Variables

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for mediawiki

# The version (major.minor.release) to install.
mediawiki_major: 1
mediawiki_minor: 31
mediawiki_release: 1

mediawiki_version: "{{ mediawiki_major }}.{{ mediawiki_minor }}.{{ mediawiki_release }}"

# Where to install mediawiki. You can use this pattern to install to a default
# location that differs per distribution, see `vars/main.yml`:
# "{{ _mediawiki_destination[ansible_distribution] | default(_mediawiki_destination['default']) }}"
# Change this to a simple string that refers to a path, for example:
# "/data/mediawiki".

mediawiki_destination: "{{ _mediawiki_destination[ansible_distribution] | default(_mediawiki_destination['default']) }}"
```

## Requirements

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.core_dependencies
- robertdebock.buildtools
- robertdebock.epel
- robertdebock.python_pip
- robertdebock.php
- robertdebock.mysql
- robertdebock.httpd

```

## Dependencies

Most roles require some kind of preparation, this is done in `molecule/default/prepare.yml`. This role has a "hard" dependency on the following roles:

- robertdebock.httpd
## Context

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/mediawiki.png "Dependency")

## Compatibility

This role has been tested on these [container images](https://hub.docker.com/):

|container|tags|
|---------|----|
|el|8|
|debian|buster, bullseye|
|fedora|31, 32|
|ubuntu|focal, bionic|

The minimum version of Ansible required is 2.8 but tests have been done to:

- The previous version, on version lower.
- The current version.
- The development version.

## Exceptions

Some variarations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| centos:7 | PHP is too old for mediawiki 1.31.1. |

## Included version(s)

This role [refers to a version](https://github.com/robertdebock/ansible-role-mediawiki/blob/master/defaults/main.yml) released by MediaWiki. Check the released version(s) here:
- [mediawiki](https://www.mediawiki.org/wiki/Download).

This version reference means a role may get outdated. Monthly tests occur to see if [bit-rot](https://en.wikipedia.org/wiki/Software_rot) occured. If you however find a problem, please create an issue, I'll get on it as soon as possible.
## Testing

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-mediawiki) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-mediawiki/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

## License

Apache-2.0


## Author Information

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
