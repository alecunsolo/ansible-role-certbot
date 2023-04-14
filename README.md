[![build status](https://github.com/alecunsolo/ansible-role-certbot/actions/workflows/ci.yml/badge.svg)](https://github.com/alecunsolo/ansible-role-certbot/actions/workflows/ci.yml)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)

Ansible Role: certbot
=========

A minimal role to install certbot and cloudflare DNS plugin.

Requirements
------------

In RHEL distribution the needed packages are in the `epel` repository.

Role Variables
--------------

```yaml
certbot_config_file: cli.ini.j2
```
Default template file used for the global certbot configuration: can be overwritten if needed.
```yaml
certbot_keytype: ecdsa
certbot_curve: secp384r1
certbot_mail: TBD
```
Configuration values used in the default configuration files.
```yaml
certbot_hooks: []
# - name: restart-nginx.sh
#   type: deploy
```
List of template files containing renewal hooks. The type is one of: `deploy`, `pre`, `post`.
```yaml
certbot_cloudflare_enabled: true
```
Whether to instal and configure the cloudflare plugin
```yaml
certbot_cloudflare_config_dir: /root/.secrets
```
The parent direcotry for the cloudflare plugin configuration file. The filename is `cloudflare.ini`
```yaml
certbot_cloudflare_token: TBD
```
The Cloudflare token used in the DNS challenge.

Distro specific variables:
```yaml
# Debian
certbot_package: certbot
certbot_cloudflare_package: python3-certbot-dns-cloudflare
certbot_renew_timer: certbot.timer
# RHEL
certbot_package: certbot
certbot_cloudflare_package: python3-certbot-dns-cloudflare
certbot_renew_timer: certbot-renew.timer
```

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- name: Converge
  hosts: all
  vars:
    certbot_mail: certbotmail@example.com
    certbot_cloudflare_token: supersecrettoken
  tasks:
    - name: Include certbot.
      ansible.builtin.include_role:
        name: alecunsolo.certbot
```

License
-------

MIT

Notes
-----

Testing with molecule is ~~stolen from~~ heavily inspired by [Jeff Geerling](https://www.jeffgeerling.com/). Watch [his video](https://youtu.be/FaXVZ60o8L8) (and the other ones as well).
