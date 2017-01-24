Ansible Users Role - ansible-users
==================================

**Ansible Role used to manage users, groups, ssh authorized keys and sudo.**

The configuration format of this role is heavily inspired by the cloud-init
cloud config. The goal is to use the same yaml structure wherever possible, yet
possibly extend it where it makes sense.

Some minor noteworthy differences:

- Any tilda characters had to be replaced by underscore; due to how ansible
  interprets variable names.

For more information aboud cloud-init, see cloud-init documentation at:  
https://cloudinit.readthedocs.io/en/latest/index.html

## Role Configuration

These values define the behaviour of the role.

- users_default_shell: (default: /bin/bash)  
The default shell used for all users. This is used as a default value if any
user has its shell attribute undefined.

- users_create_primary_group: (default: true)  
If the role should create groups if they do not exist. The default is to do this
to prevent the role from failing. Disable this if managed elsewhere.

- users_enable_sudo: (default: true)  
Enables support for granting a user sudo privileges, as is part of the
cloud-init configuration format. This is made optional in case a host
does not have sudo installed and that should remain the case.

## Using the Role

### Managing Users

This role is rather flexible and so the only mandatory attribute is the name.

Definable values:

- name - The username of the user.
- gecos - The comment field, also known and used for the real name of the user.
- homedir - The home directory of the user.
- primary_group - The primary user group. 
- groups - A list of complementary groups for the user.
- no_create_home - If true, do not create a home directory.
- shell - The default user shell.
- ssh_authorized_keys - A list of ssh public keys to add to add to an
  authorized_keys file.
- sudo - The sudo string that will be used to configure sudo if
  users_enable_sudo is true.
- system - If true, the user will be a system user. This does not affect
  existing users. It also means no home directory is created.

Example:
```
users_default_shell: /bin/bash
users_create_primary_group: true
users_enable_sudo: true

users:
  - name: foobar
    gecos: Foo B. Bar
    primary_group: foobar
    groups: ['users','wheel']
    shell: /bin/sh
    ssh_authorized_keys:
      - "ssh-rsa AAAAA.... foo@machine"
      - "ssh-rsa AAAAB.... bar@machine"
    sudo: ALL=(ALL) ALL

  - name: foobar
    gecos: FooBar Service Account
    homedir: /
    primary_group: foobar
    shell: /sbin/nologin
    system: true
```

### Deleting Users

Simply move over your users to the users_deleted list. You may remove all
lines except **"name"** which is mandatory.

It may however be wise to leave the entries for history/archival purposes, or
in case you need to recreate a user. Password entries are best left out.

Example 1:
```
users_deleted:
  - name: foobar1
  - name: foobar2
```

Example 2:
```
users_deleted:
  - name: foobar
    gecos: Foo B. Bar
    primary_group: foobar
    groups: ['users','wheel']
    shell: /bin/sh
    ssh_authorized_keys:
      - "ssh-rsa AAAAA.... foo@machine"
      - "ssh-rsa AAAAB.... bar@machine"
    sudo: ALL=(ALL) ALL
```
