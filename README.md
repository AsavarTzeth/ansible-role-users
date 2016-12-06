Ansible Users Role - ansible-users
==================================

**Ansible Role used to manage users, groups and ssh authorized keys.**

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

## Using the Role

This role is rather flexible and so the only mandatory attribute is the name.

Definable values:

- name - The username of the user.
- gecos - The comment field, also known and used for the real name of the user.
- primary_group - The primary user group. 
- groups - A list of complementary groups for the user.
- shell - The default user shell.
- ssh_authorized_keys - A list of ssh public keys to add to add to an
  authorized_keys file.

Added values, outside of cloud-init:

- state - This does exactly what an ansible user would expect.
It is used to define if a user should exist or not.
(Choices: present, absent)[Default: present]

Example:
```
users_default_shell: /bin/bash
users_create_primary_group: true

users:
  - name: foobar
    gecos: Foo B. Bar
    primary_group: foobar
    groups: ['users','wheel']
    shell: /bin/sh
    ssh_authorized_keys:
      - "ssh-rsa AAAAA.... foo@machine"
      - "ssh-rsa AAAAB.... bar@machine"
    state: present
```
