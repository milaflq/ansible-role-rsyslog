# [rsyslog](#rsyslog)

Install and configure rsyslog on your system.

|GitHub|GitLab|Quality|Downloads|Version|Issues|Pull Requests|
|------|------|-------|---------|-------|------|-------------|
|[![github](https://github.com/buluma/ansible-role-rsyslog/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-rsyslog/actions)|[![gitlab](https://gitlab.com/buluma/ansible-role-rsyslog/badges/master/pipeline.svg)](https://gitlab.com/buluma/ansible-role-rsyslog)|[![quality](https://img.shields.io/ansible/quality/58234)](https://galaxy.ansible.com/buluma/rsyslog)|[![downloads](https://img.shields.io/ansible/role/d/58234)](https://galaxy.ansible.com/buluma/rsyslog)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-rsyslog.svg)](https://github.com/buluma/ansible-role-rsyslog/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-rsyslog.svg)](https://github.com/buluma/ansible-role-rsyslog/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-rsyslog.svg)](https://github.com/buluma/ansible-role-rsyslog/pulls/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/default/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: buluma.rsyslog
```

The machine needs to be prepared. In CI this is done using `molecule/default/prepare.yml`:
```yaml
---
- name: Prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: buluma.bootstrap
```


## [Role Variables](#role-variables)

The default values for the variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for rsyslog

# To configure a server to receive logs, set rsyslog_receiver to yes.
rsyslog_receiver: no

# To forward logs to another server, set rsyslog_remote to the hostname or
# the ipaddress of the receiving rsyslog server.
# Not setting this variable will not forward logs.
# rsyslog_remote: server1.example.com

# If rsylog_remote is set, sets the "selector" pattern for determining which
# messages to send to the remote server.  Default "*.*" sends everything.
# See `man rsyslog.conf`.
rsyslog_remote_selector: "*.*"

# If rsylog_remote is set, use TCP if yes. UDP if no.
rsyslog_remote_tcp: yes

# If rsylog_remote is set, destination port to use.
rsyslog_remote_port: 514

# Set the mode for new directories; only available in legacy template.
rsyslog_dircreatemode: "0700"

# Set the mode for new files; only available in legacy template.
rsyslog_filecreatemode: "0644"

# Set the mods enabled
rsyslog_mods:
  - imuxsock
  - imjournal

# Configure rsyslog minimally (may be in conflict with custom configuration files)
rsyslog_deploy_default_config: yes

# Default rsyslogd rules
rsyslog_default_rules:
  - { rule: '*.info;mail.none;authpriv.none;cron.none', logpath: '/var/log/messages' }
  - { rule: 'authpriv.*', logpath: '/var/log/secure' }
  - { rule: 'mail.*', logpath: '-/var/log/maillog' }
  - { rule: 'cron.*', logpath: '/var/log/cron' }
  - { rule: '*.emerg', logpath: ':omusrmsg:*' }
  - { rule: 'uucp,news.crit', logpath: '/var/log/spooler' }
  - { rule: 'local7.*', logpath: '/var/log/boot.log' }

# Use the (obsolete) legacy, pre-v6 configuration file format, or the more
# modern # 'advanced' configuration file format available in v6 and up. The
# default is to use the 'legacy' format to not change config files for
# existing users of this role out of the blue.
# More information on the different formats on the rsyslog website:
# https://www.rsyslog.com/doc/v8-stable/configuration/conf_formats.html
rsyslog_config_file_format: legacy

# The rule conf to name to add to /etc/rsyslog.d/
# rsyslog_forward_rule_name: <to fill>

# Configure the rsyslog package to be `present`, or set to `latest` to install
# the latest available version.
rsyslog_package_state: present

# if set to false (legacy default to remain compatible to sysklogd), the domain
# part from a name that is within the same domain as the receiving system is
# stripped. If set to on, full names are always used.
rsyslog_preservefqdn: no

# Configure additional config files in /etc/rsyslog.d
# Example:
# rsyslog_rsyslog_d_files:
#   000-splunk:
#     content: |
#       auth,authpriv.* action(type="omfwd"
#                              target="splunk"
#                              port="514"
#                              protocol="tcp")
rsyslog_rsyslog_d_files: []

# Avoid sd_journal_get_cursor() failed: Cannot assign requested address messages
# due to logrotate
rsyslog_workaroundjournalbug: no

# Turns off message reception via local log socket
rsyslog_imuxsock_syssock: no

# Install additional rsyslog packages
# Example for installing rsyslog-elastisearch:
# rsyslog_features:
#   - gnutls
#   - elastisearch
rsyslog_features: []
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-rsyslog/blob/main/requirements.txt).

## [Status of used roles](#status-of-requirements)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|[![Build Status GitLab ](https://gitlab.com/buluma/ansible-role-bootstrap/badges/main/pipeline.svg)](https://gitlab.com/buluma/ansible-role-bootstrap)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-rsyslog/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|alpine|all|
|amazon|Candidate|
|el|8|
|debian|all|
|fedora|all|
|opensuse|all|
|ubuntu|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

## [Exceptions](#exceptions)

Some roles can't run on a specific distribution or version. Here are some exceptions.

| variation                 | reason                 |
|---------------------------|------------------------|
| archlinux/base | target not found: rsyslog |


If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-rsyslog/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-rsyslog/blob/master/CHANGELOG.md)

## [License](#license)

Apache-2.0

## [Author Information](#author-information)

[Michael Buluma](https://buluma.github.io/)
