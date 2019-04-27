## What is ansible-sopel? [![Build Status](https://travis-ci.com/sopel-irc/ansible-sopel.svg?branch=master)](https://travis-ci.com/sopel-irc/ansible-sopel)

It is an [Ansible](http://www.ansible.com/home) role that installs sopel irc bot in a virtual environment.
## Supported platforms

- Ubuntu 16.04 LTS (Xenial)
- Ubuntu 18.04 LTS (Bionic)
- Debian 8 (Jessie)
- Debian 9 (Stretch)
- CentOS 7

## Role variables

``` yaml
# The install dir is also the home dir of the user created for sopel
sopel_install_dir: '/srv/sopel'
sopel_config_dir: '/etc/sopel'
sopel_log_dir: '/var/log/sopel'
sopel_pid_dir: '/run/sopel'

sopel_install_systemd_service: true

# If you do not want this role to install python3, set this to false.
# Python3 and venv is still required, but you can install it yourself
sopel_install_python3: true

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

# Default timezone and time format. http://strftime.org/ for format info
sopel_timezone: 'Europe/Copenhagen'
sopel_time_format: '[%Y-%m-%d - %T %Z]'

```

## Example usage

``` yaml
---
- name: Install Sopel
  hosts: vps
  become: true
  tags:
    - sopel

  vars:
    sopel_auth_method: 'nickserv'
    sopel_bot_owner: 'testman'
    sopel_auth_user: authUser
    sopel_auth_pass: authPass

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
