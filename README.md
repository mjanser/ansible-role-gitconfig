# Ansible Role: gitconfig

An Ansible role that configures git for various users.

## Requirements

None

## Role Variables

Available variables are listed below, along with default values:

    gitconfig_users: []

Possible values for each entry in `gitconfig_users` are, along with default values:

    user: ~
    name: ~
    email: ~
    ignore_github: []
    ignore_custom: ~
    push_default: simple
    fetch_prune: yes

### User information

The key `user` defines the username which this configuration should be applied to.
The configuration will be stored in `/home/{user}`.

With the keys `name` and `email` the corresponding git config parameters can be set.

### Global ignore

A global gitignore list can be configured using GitHub files and a custom list.
The key `ignore_github` can contain a list of file names which can be found on https://github.com/github/gitignore/tree/master/Global.

With the key `ignore_custom` you can define a custom gitignore list.

### Push default

The default behaviour for pushing can be defined in the key `push_default` which defaults to `simple`.
See https://git-scm.com/docs/git-config.html#git-config-pushdefault for more information.

### Fetch prune

By default git will be configured to add `--prune` when fetching from a remote.
This can be disabled with setting `fetch_prune` to `no`.
See https://git-scm.com/docs/git-config.html#git-config-fetchprune for more information.

## Dependencies

None

## Example Playbook

    - hosts: all
      roles:
        - { role: mjanser.gitconfig }
      vars:
        gitconfig_users:
          - user: vagrant
            name: Vagrant User
            email: info@example.com
            ignore_github:
              - Linux
              - Vim
            ignore_custom: |
              .vagrant
              *.bak

## License

MIT
