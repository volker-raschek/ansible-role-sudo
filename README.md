# volker-raschek.sudo

![Ansible Role](https://img.shields.io/ansible/role/d/volker-raschek/sudo)

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

## Tests

The role is tested with [Molecule](https://ansible.readthedocs.io/projects/molecule/). The scenario starts one docker
container per supported distribution family, applies the role, asserts that a second run reports no change and finally
verifies the created drop-in files, their permissions and content, that a rule declared as `absent` is gone again and
that `visudo` accepts the resulting configuration.

Molecule ships only its `default` driver, therefore `docker` is required beside molecule itself. The collections are
declared in `molecule/default/collections.yml` and installed by molecule.

```bash
pip install molecule docker
```

The complete sequence creates the containers, tests them and removes them afterwards.

```bash
molecule test
```

While working on the role the containers are better kept alive.

```bash
# create the containers and apply the role
molecule converge

# run the assertions of molecule/default/verify.yml against the running containers
molecule verify

# open a shell in one of the containers
molecule login --host sudo-debian

# remove the containers
molecule destroy
```

## Configuring

In the default directory are examples how to configure the network stack. Copy the
defaults into your `host_vars` or `group_vars` and adapt the examples.
