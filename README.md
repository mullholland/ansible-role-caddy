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
          import tls
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
      ansible.builtin.copy:
        dest: /etc/ssl/key.pem
        owner: foo
        group: foo
        mode: '0644'
        content: |
          -----BEGIN PRIVATE KEY-----
          MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDBJpbKyzg9MX2v
          criaHyM85O6W4s/Ao0Bz+98tBYPBeyZ2vdrPAIt+EGCPU/tRvyMw5/ZJCXfZiJuD
          GbFzppDh24JGcDftvhEH+N4pokefLFu5xZ7MlDU4IbB88ggP+fIjiaYvVENS396t
          kLVGEXWUJH/DAv836SSiDMTSnggJUAnRx94imqd4hYvzYdDTfCgbRXRqGr6wghsu
          vXrkGYlaWtE2P9GreJEpChVtcMsHBfUgYG0GfbV1z8l7L2aqn8WiLLGrb6Lifppv
          Xoqw1A92JoEAu4UekpJZIdIgDHsFj3Z84c/RcyCcjlXTsLMfDOOGFfhgYALNHEBq
          y1TgNOsPAgMBAAECggEAAalG+L2LYUiwr7aejIIiDR8G8k3xwtK59jAUuPq9fweD
          yten2cnuaTTThR1ldvbcOEp2ctBdszBFQ3kQGVI1wsuJhk48HOkFP8/4e9vac9ha
          KEc2g29EOemy7pAtA5N/F/vSGGwdcXQIIplbsHDsAEyEEMr7ePaiCwbDFptSBF/O
          WWe/7iN8m/ZZYHTz5mZg4vXen4EJrK57KP3VPnKbllFPBvDa1VdVYpxIqUui2SDR
          /PWkkn88pn4PiPHQXMio6dy4zk9jB6ptXZp+9uptkMGoXeCP8eRBsqXIaMZ4hcJy
          XQWNvCweCItohb+xFd5ECj1cWeosrTEviWuOjf/n2QKBgQDy/j+mHkOrsdty3fBF
          J3t3AYT6vnmF09QP6Uwn3+rgVutwg5f8mVZZQaOoW+yELkCOZIFAKq924voLsCBe
          0bukpApHDpCquj6gw7WvgjQ01YT+5erynp4Jg3LZjSRoH6qp2rxbx53HKTPVJWJH
          4xwhsqAoN3Z9oTgbrb/qq5QSfQKBgQDLfViudFSGPrxDG1l2xXYKREBv9pIAhWig
          x/GYhzOj5mpgbuL6EoMUp9C6xWJM26WOf3vGAGCSQ3yPdy6IGwnPnFnTDqDlgy3L
          VNMjgsUuHF3H9IuejmWoXgTIezFc0q5lBX7iOSJQN87kEpSWbb1TiT+mZwvEvXB0
          GCN/TH19ewKBgQDvqtYcgr08G7DXGxBhJRAh0N3YcwZpeQUwrGrw6WpA23pc/25p
          NtR0NMm2xPQDa5tA1uCk6XUnTbhSzuUeoL7zJNj+PN9zhT9AUchh04qqke8beqrB
          orE9sOkWqp++E33BCn2+CKUWSw1UrgrB3L9ifUx6XjoAr4MnybgBPjpOAQKBgE7f
          RNJJsMFf66SvIxwQKVKNZdR/49Nj4kv/c7tFHFT46F58XGnFZx1IdnUOMK3NrPvw
          mc8DMms+0TbiYRzMLh9UYNSXpPGQyN05AaWP+FGJGSh5tuw8EVcTKhNy/I0X9BSf
          7rBMqOoi14Q7V3B/FJUea5dZ9YvKSZ4WBRxAT5ulAoGBAL0OzzhSspM61X9plBMn
          nLXnwDBYKrN/HyhV0aIRanPobp+w8QvLVSQIx7ReNTTr/JfOPh3+Y+kpjDOqWB/A
          ocu0oiJTexAp6Tuk8Yhyle8YJO8HOSGrnNZiCKg1xepH7E/nOfE8FACAIsgWDVY5
          Z/NXxvI+xUyJSUiEpFafMfCV
          -----END PRIVATE KEY-----
    - name: Create simple self-signed certificate
      ansible.builtin.copy:
        dest: /etc/ssl/cert.pem
        owner: foo
        group: foo
        mode: '0644'
        content: |
          -----BEGIN CERTIFICATE-----
          MIIDkzCCAnugAwIBAgIUA8DUmIJmFWnVblVzrJ1ubXFc35AwDQYJKoZIhvcNAQEL
          BQAwWTELMAkGA1UEBhMCREUxEzARBgNVBAgMClNvbWUtU3RhdGUxEjAQBgNVBAcM
          CUZyYW5rZnVydDEhMB8GA1UECgwYSW50ZXJuZXQgV2lkZ2l0cyBQdHkgTHRkMB4X
          DTIzMTIwOTAwMDYyOFoXDTMzMTIwNjAwMDYyOFowWTELMAkGA1UEBhMCREUxEzAR
          BgNVBAgMClNvbWUtU3RhdGUxEjAQBgNVBAcMCUZyYW5rZnVydDEhMB8GA1UECgwY
          SW50ZXJuZXQgV2lkZ2l0cyBQdHkgTHRkMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A
          MIIBCgKCAQEAwSaWyss4PTF9r3K4mh8jPOTuluLPwKNAc/vfLQWDwXsmdr3azwCL
          fhBgj1P7Ub8jMOf2SQl32Yibgxmxc6aQ4duCRnA37b4RB/jeKaJHnyxbucWezJQ1
          OCGwfPIID/nyI4mmL1RDUt/erZC1RhF1lCR/wwL/N+kkogzE0p4ICVAJ0cfeIpqn
          eIWL82HQ03woG0V0ahq+sIIbLr165BmJWlrRNj/Rq3iRKQoVbXDLBwX1IGBtBn21
          dc/Jey9mqp/Foiyxq2+i4n6ab16KsNQPdiaBALuFHpKSWSHSIAx7BY92fOHP0XMg
          nI5V07CzHwzjhhX4YGACzRxAastU4DTrDwIDAQABo1MwUTAdBgNVHQ4EFgQUOzrA
          K933vRYSNIxM+C5J0ksCw/cwHwYDVR0jBBgwFoAUOzrAK933vRYSNIxM+C5J0ksC
          w/cwDwYDVR0TAQH/BAUwAwEB/zANBgkqhkiG9w0BAQsFAAOCAQEAEOIbiQfSriAC
          Rf8WKAYMHuOH7P2eX7wOYVUNphX+VDD70AKjsGAZSRdqg6G1suaGjP/HGc1+GYto
          vSPMwt68nzgiBe0rQDaiv0Zvig7Y+Uiyiq4oJ4oO8xtcB51THVERlxa8FW+jPmCP
          WAor/8PPAXcccCP2E9iNvs4vyZscenrJdv6FfyQwevU8t2BUm4y6M8ZnwYMizc+/
          bfemCpAYKfNhmnnfu4UjUwr2GC5Dj87I2lm9ennNwYJhl1/9FSPtI3RcE+6QHxYI
          4ewHSo7TX1rU+oF3NkxbqvqXc4xMIby4ll2Op5Ua3ZahTtIimHly2x7t3QiU4xeI
          mqZ30O3l3A==
          -----END CERTIFICATE-----

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
