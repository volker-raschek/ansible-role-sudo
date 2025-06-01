# sudo_users

[![Ansible Role](https://img.shields.io/ansible/role/d/58433)](https://galaxy.ansible.com/volker_raschek/networking_role)

With following role can be created sudoers files in `/etc/sudoers.d`. For
example to grant a user special perimssions to execute a program as root.

## Supported distributions

- Arch Linux
- Debian
- Fedora
- RHEL
- Ubuntu 20.04

## Features

- Installing sudo
- Configuring drop-on files in `/etc/sudoers.d`

## Configuring

In the default directory are examples how to configure the network stack. Copy the
defaults into your `host_vars` or `group_vars` and adapt the examples.
