# Ansible Role: gitconfig

An Ansible role that configures git for various users.

## Requirements

None

## Role Variables

Available variables are listed below, along with default values:

    gitconfig_users: []

Each entry in the list can have these keys:

    user: ~
    name: ~
    email: ~
    ignore_github: []
    ignore_custom: ~ # string
    push_default: upstream
    fetch_prune: true

The items in `ignore_github` are fetched from https://github.com/github/gitignore/tree/master/Global.

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
