uclalib_role_users [![Build Status](https://travis-ci.org/UCLALibrary/uclalib_role_users.svg?branch=master)](https://travis-ci.org/UCLALibrary/uclalib_role_users)
=========

Create System Groups and Users.

Requirements
------------

None.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

### Groups to Create
`local_groups` is a list of names and (optional) GIDs for groups to create. If GID is not specified, will use system default
```yaml
local_groups:
  - name: admin
    gid: 501
  - name: badmin
```

### Users to Create
`local_users` is a list of names, with optional uid, fullname, homedir, shell, primary group, supplemental groups, ssh authorized keys. Omitted items will be defiend as per system dfaults.
`groups` is a list of supplemenal groups
`authorized_keys` is a list of ssh authorized keys, either as the contents of a public key, or an URI
```yaml
local_users:
  - name: alice
  - name: bob
    fullname: Bob
    homedir: /home/bob.local
    shell: /usr/bin/zsh
    group: users
    groups:
      - wheel
      - adm
    authorized_keys:
      - ecdsa-sha2-nistp256 [key] bob@secure
      - https://github.com/user.keys
      - https://gitlab.com/user.keys
```

### Passwords to Apply

`local_passwords` is a list of names and crypted passwords. The users must already exist.

```yaml
local_passwords:
  - name: alice
    pwhash: '$1$wZpERDHA$c1q6Q/mOWbXMPCza4NpWK1'
```

### SSH Private Keys to Install

`local_ssh_keys` is a list of users, destinations, and keys to install. The user must already exist.

```yaml
local_ssh_keys:
  - name: bob
    keyname: ~/.ssh/id_rsa
    sshkey: |
      -----BEGIN RSA PRIVATE KEY-----
      MIIEowIBAAKCAQEAwdXng5UD/Gy83ZFSJrwjNS3WzRUXGzL98B3PeVcjOSVM34tX
      ...
      -----END RSA PRIVATE KEY-----
```

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- name: Install Users and Keys
  hosts: all
  vars:
    local_groups:
      - name: admin
        gid: 503
      - name: staff
    local_users:
      - name: alice
        fullname: Alice Doe
      - name: bob
        fullname: Robert Smith
        shell: /bin/zsh
    local_passwords:
      - name: vagrant
        pwhash: '$1$gPNBpA.5$5pr.KtXhOx6S/Hc69TUZZ.'
    local_ssh_keys:
      - name: alice
        keyname: id_rsa
        sshkey: '{{ vault_alice_ssh_key }}'

  roles:
    - name: uclalib_role_users
      become: true
```

License
-------

BSD 3-Clause

Author Information
------------------

- Anthony Vuong
- John H. Robinson, IV
