[![Build Status](https://travis-ci.org/AsavarTzeth/ansible-role-users.svg?branch=master)](https://travis-ci.org/AsavarTzeth/ansible-role-users)

Ansible Users Role - ansible-role-users
==================================

**Ansible role used to manage users, groups, ssh authorized keys and sudo.**

The configuration format of this role is heavily inspired by the format used
with cloud-init/cloud config. The goal is to use the same or similar yaml
structure wherever possible, but possibly extend it where it makes sense.

Some minor noteworthy differences:

- Any dash characters had to be replaced by underscore; due to how ansible
  interprets variable names.

For more information aboud cloud-init, see cloud-init documentation at:  
https://cloudinit.readthedocs.io/en/latest/index.html

Requirements
------------

This role was developed and tested on Ansible 2.2.0 and higher.
It may work on lower versions but that is currently unsupported.

Role Variables
--------------

List of variables and default values:

    # The default user shell, used on user creation.
    users_default_shell: /bin/bash

    # Defines if the role should create a primary group if it does not exist.
    # In order to prevent the role from failing this is set to true by default.
    # Disable this if groups are managed elsewhere.
    users_create_primary_group: true

    # Enables management of privilege escalation using sudo.
    # Disable this if sudo will not be used, or is managed elsewhere.
    users_enable_sudo: true


    # The only mandatory parameter is the name.
    users:
      - name: ''                           # The username of the user.
        gecos: ''                          # The comment field, also known and used for the real name of the user.
        homedir: ''                        # The home directory of the user.
        primary_group: ''                  # The primary user group.
        groups: []                         # A list of complementary groups for the user.
        no_create_home: false              # If true, do not create a home directory. Defaults to true if `system: true`.
        shell: "{{ users_default_shell }}" # The default user shell.
        passwd: ''                         # A SHA512 hashed and salted password.
        ssh_authorized_keys: []            # A list of ssh public keys to add to add to an authorized_keys file.
        sudo: ''                           # The sudo string that will be used to configure sudo.
        system: false                      # If true, the user will be a system user. This does not affect existing users.

Dependencies
------------

None

Example Playbook
----------------

Add or modify a user and set up sudo and ssh authorized keys:

    - hosts: all
      roles:
        - role: AsavarTzeth.users
          users_default_shell: /bin/bash
          users_create_primary_group: true
          users_enable_sudo: true
          users:
            - name: foobar1
              gecos: Foo B. Bar
              primary_group: foobar1
              groups: ['users','wheel']
              shell: /bin/bash
              ssh_authorized_keys:
                - "ssh-rsa AAAAA.... foo@host"
                - "ssh-rsa AAAAB.... bar@host"
              sudo: ALL=(ALL) ALL

Add or modify a system user:

    - hosts: all
      roles:
        - role: AsavarTzeth.users
          users:
            - name: foobar2
              gecos: FooBar Service Account
              homedir: /
              primary_group: foobar
              shell: /sbin/nologin
              system: true

Deleting users:

    - hosts: all
      roles:
        - role: AsavarTzeth.users
          users_deleted:
            - name: foobar1
            - name: foobar2

Modifying a user password:

    - hosts: all
      roles:
        - role: AsavarTzeth.users
          users:
            - name: foobar1
              passwd: $6$mI3A2y4O.YfqhlPt$szsWfnICXsYLbsIghLauJG.I3enLYGDPBYO1DYTHn9gB6y3Q2faM7iqievJlU5ZMTT9X3wHrUv0c7HWkToGBp/

License
-------

BSD-2-Clause

Author Information
------------------

Patrik Nilsson
