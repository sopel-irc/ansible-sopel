# *Deprecated. This role is no longer maintained. It is recommended to use the [officially unoffical docker image](https://github.com/sopel-irc/docker-sopel) instead.*
## What is ansible-sopel? [![CI](https://github.com/sopel-irc/ansible-sopel/workflows/CI/badge.svg)](https://github.com/sopel-irc/ansible-sopel/actions?query=workflow%3ACI)

It is an [Ansible](http://www.ansible.com/home) role that installs sopel irc bot in a virtual environment.
## Supported platforms

- Ubuntu 18.04 LTS (Bionic)
- Ubuntu 20.04 LTS (Focal)
- Debian 9 (Stretch)
- Debian 10 (Buster)
- CentOS 7
- CentOS 8

## Dependencies
- Python 3
- Python venv
- Python wheel or build tools

## Role variables

``` yaml
---
# Changing the instance name will allow several instances of Sopel to
# run side-by-side on the same server as long as they have
# different nicks, or connect to different servers.
sopel_instance_name: 'sopel'
sopel_install_dir: '/srv/sopel'
sopel_config_dir: '/etc/sopel'
sopel_log_dir: '/var/log/sopel'
sopel_pid_dir: '/run/sopel'

sopel_install_systemd_service: true
sopel_start_systemd_service: true

# If your system uses a different virtual environment wrapper you can overwrite the venv command
sopel_venv_cmd: '/usr/bin/python3 -m venv'

# The prefix used to call the bot.
# It's parsed as regex so remember to escape special characters
sopel_command_prefix: '\.'

# The nick sopel will present itself as in channels
sopel_nick: 'sopel_irc_bot'
sopel_auth_method: 'sasl'

# The network sopel should connect to
sopel_irc_host: 'chat.freenode.org'
sopel_irc_port: 6697

# A list of channels to join
sopel_channels:
  - '##botspam'

# The admin of the bot
sopel_bot_owner: ''

# A list of nicks and hostmasks for sopel to ignore. Parsed as regex
sopel_ignored_nicks:
  - ''
sopel_ignored_hosts:
  - ''

# A list of plugins to enable. Default is everything enabled
#sopel_enabled_plugins: []

# A list of plugins to exclude. Default is none excluded
#sopel_excluded_plugins: []

## Any further additions to the sopel config can be added through this variable
## it is appended to the end of the config
#sopel_config_extra: |
#  [currency]
#  auto_convert = true

# Default timezone and time format. http://strftime.org/ for format info
sopel_timezone: 'Europe/Copenhagen'
sopel_time_format: '[%Y-%m-%d - %T %Z]'
```

## Example usage

Example showing how to quickly and easily deploy two instances of sopel
This will set up two sopel instances, one with the default name sopel and one called sopel2
It'll also install them as systemd services under the names: sopel-sopel and sopel-sopel2
the config files will be found in /etc/sopel/
``` yaml
---
- name: 'Install Sopel instance 1'
  hosts: vps
  become: true
  tags:
    - sopel

  vars:
    sopel_auth_method: 'nickserv'
    sopel_bot_owner: 'testManDan'
    sopel_nick: Sopel_bot_1
    sopel_auth_user: NICK OWNER HERE
    sopel_auth_pass: NICK PASS HERE

  roles:
   - sopel.sopel

- name: 'Install Sopel instance 2'
  hosts: vps
  become: true
  tags:
    - sopel

  vars:
    sopel_instance_name: 'sopel2'
    sopel_auth_method: 'nickserv'
    sopel_bot_owner: 'testManDan'
    sopel_nick: Sopel_bot_2
    sopel_auth_user: NICK OWNER HERE
    sopel_auth_pass: NICK PASS HERE

  roles:
    - sopel.sopel
```

## Installation

`$ ansible-galaxy install sopel.sopel`

## Ansible Galaxy

You can find it on the official
[Ansible Galaxy](https://galaxy.ansible.com/sopel/sopel/) if you want to
rate it.

## License

MIT

---
**Thanks to Geerlingguy for great ansible CI documentation. CI is adapted from his work.**

