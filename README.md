# volker-raschek.sudo

![Ansible Role](https://img.shields.io/ansible/role/d/volker-raschek/sudo)

Installs sudo and manages individual rules in `/etc/sudoers.d` for users and groups. Unmanaged drop-in files are
preserved.

## Requirements

Install the required collection before running the role:

```bash
ansible-galaxy collection install -r requirements.yml
```

The role requires gathered facts to select distribution-specific package names and elevated privileges to install
packages and configure sudo. Arch Linux, Debian, Fedora, RHEL and Ubuntu are supported.

## Parameters

| Name | Description | Default |
| --- | --- | --- |
| `sudo_users_sudoers` | List of sudoers drop-ins. Each entry must specify exactly one of `user` and `group`. | `[]` |

Each list entry accepts these keys:

| Key          | Description                                                    | Default             |
|--------------|----------------------------------------------------------------|---------------------|
| `user`       | User to grant permissions to. Mutually exclusive with `group`. | None                |
| `group`      | Group to grant permissions to. Mutually exclusive with `user`. | None                |
| `filename`   | Name of the drop-in file.                                      | User or group name  |
| `commands`   | Command or list of commands allowed by the rule.               | `ALL`               |
| `nopassword` | Allow execution without a password.                            | `false`             |
| `runas`      | User to run commands as.                                       | sudo default (root) |
| `state`      | `present` creates the rule; `absent` removes its drop-in.      | `present`           |

## Example

```yaml
- name: Configure sudo
  hosts: all
  become: true
  roles:
    - role: volker-raschek.sudo
      sudo_users_sudoers:
        - user: alice
          commands:
            - /usr/bin/systemctl restart nginx
          nopassword: true
        - group: operators
          filename: operators-as-root
          runas: root
          commands:
            - /usr/bin/id
        - user: former-user
          state: absent
```

## Tests

The [Molecule](https://ansible.readthedocs.io/projects/molecule/) scenario tests Arch Linux, Debian, Fedora and Ubuntu
containers. It checks idempotence, user and group rules, custom filenames, removal, preservation of unmanaged files,
permissions, invalid entries and `visudo` validation.

Install Molecule and the Docker Python SDK, and ensure the Docker daemon is accessible:

```bash
pip install molecule docker
molecule test
```

For interactive testing:

```bash
molecule converge
molecule verify
molecule login --host sudo-ubuntu
molecule destroy
```
