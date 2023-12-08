# [Ansible role caddy](#caddy)

Install and configures the caddy webserver.

|GitHub|Downloads|Version|
|------|---------|-------|
|[![github](https://github.com/mullholland/ansible-role-caddy/actions/workflows/molecule.yml/badge.svg)](https://github.com/mullholland/ansible-role-caddy/actions/workflows/molecule.yml)|[![downloads](https://img.shields.io/ansible/role/d/mullholland/caddy)](https://galaxy.ansible.com/mullholland/caddy)|[![Version](https://img.shields.io/github/release/mullholland/ansible-role-caddy.svg)](https://github.com/mullholland/ansible-role-caddy/releases/)|
## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/mullholland/ansible-role-caddy/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true
  vars:
    caddy_config_snippets:
      - name: tls
        config: |
          /etc/ssl/cert.pem /etc/ssl/key.pem
    caddy_config_sites:
      - name: example.com
        config: |
          root * /var/www
          file_server

  roles:
    - role: "mullholland.caddy"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/mullholland/ansible-role-caddy/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: true

  tasks:
    - name: Create private key
      community.crypto.openssl_privatekey:
        path: /etc/ssl/key.pem
    - name: Create simple self-signed certificate
      community.crypto.x509_certificate:
        path: /etc/ssl/cert.pem
        privatekey_path: /etc/ssl/key.pem
        provider: selfsigned

  roles:
    - role: mullholland.repository_caddy
```



## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/mullholland/ansible-role-caddy/blob/master/defaults/main.yml):

```yaml
---
# systemd
caddy_service_after_units: []
caddy_service_require_units: []
caddy_env_vars: []

# Vhosts
caddy_config_globals: |
  # Enable Prometheus metrics
  servers {
    metrics
  }

caddy_config_snippets: []
#  - name: tls
#    config: |
#      /etc/ssl/cert.pem /etc/ssl/key.pem

# https://caddyserver.com/docs/caddyfile/patterns
caddy_config_sites: []
#  # Static file server
#  - name: example.com
#    config: |
#      root * /var/www
#      file_server
#  # Reverse proxy
#  # Proxy all requests
#  - name: example.com
#    config: |
#      reverse_proxy localhost:5000
#  # Only proxy requests having a path starting with /api/
#  # and serve static files for everything else
#  - name: example.com
#    config: |
#      root * /var/www
#      reverse_proxy /api/* localhost:5000
#      file_server
#  # php
#  - name: example.com
#    config: |
#      root * /srv/public
#      encode gzip
#      php_fastcgi localhost:9000
#      file_server
#  # php
#  - name: example.com
#    config: |
#      root * /srv/public
#      encode gzip
#      php_fastcgi localhost:9000
#      file_server
#  # redirect
#  # To add the www. subdomain with an HTTP redirect
#  - name: example.com
#    config: |
#      redir https://www.{host}{uri}
#  # To remove it
#  - name: example.com
#    config: |
#      redir https://example.com{uri}
#  # tls
#  # Wildcard certificates
#  - name: "*.example.com"
#    config:
#      tls {
#        dns <provider_name> [<params...>]
#      }
#
#      @foo host foo.example.com
#      handle @foo {
#        respond "Foo!"
#      }
#
#      @bar host bar.example.com
#      handle @bar {
#        respond "Bar!"
#      }
#
#      # Fallback for otherwise unhandled domains
#      handle {
#        abort
#      }
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/mullholland/ansible-role-caddy/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[mullholland.repository_caddy](https://galaxy.ansible.com/mullholland/repository_caddy)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-repository_caddy/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-repository_caddy/actions)|[![Build Status GitLab](https://gitlab.com/opensourceunicorn/ansible-role-repository_caddy/badges/master/pipeline.svg)](https://gitlab.com/opensourceunicorn/ansible-role-repository_caddy)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://mullholland.net) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/mullholland/ansible-role-caddy/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/mullholland/enterpriselinux)|all|
|[Amazon](https://hub.docker.com/r/mullholland/amazonlinux)|Candidate|
|[Fedora](https://hub.docker.com/r/mullholland/fedora/)|all|
|[Ubuntu](https://hub.docker.com/r/mullholland/ubuntu)|all|
|[Debian](https://hub.docker.com/r/mullholland/debian)|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-caddy/issues).

## [License](#license)

[MIT](https://github.com/mullholland/ansible-role-caddy/blob/master/LICENSE).

## [Author Information](#author-information)

[mullholland](https://mullholland.net)
