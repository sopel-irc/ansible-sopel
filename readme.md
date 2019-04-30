## What is ansible-sopel? [![Build Status](https://travis-ci.com/sopel-irc/ansible-sopel.svg?branch=master)](https://travis-ci.com/sopel-irc/ansible-sopel)

It is an [Ansible](http://www.ansible.com/home) role that installs sopel irc bot in a virtual environment.
## Supported platforms

- Ubuntu 16.04 LTS (Xenial)
- Ubuntu 18.04 LTS (Bionic)
- Debian 8 (Jessie)
- Debian 9 (Stretch)
- CentOS 7

Debian 8 is currently not being tested on travis, as there's an issue with the docker image used for it, however it has been tested successfully on an actual install.

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
    - sopel

```

## Installation

`$ ansible-galaxy install sopel.sopel`

## Ansible Galaxy

You can find it on the official
[Ansible Galaxy](https://galaxy.ansible.com/sopel/sopel/) if you want to
rate it.

## License

MIT
