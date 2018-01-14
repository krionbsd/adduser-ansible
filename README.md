Adduser role description:
=========================

## Introduction

This repository implements adduser configuration for the Linux
Ubuntu/CentOS enviroments.

## Local Development

The following software must be installed on your host in order to
deploy.

- Ansible
- Vagrant (optional)
- Virtualbox (optional)

Install `ansible` and the other python dependencies

```
pip install -r requirements.txt
```

## Tips and Tricks

#### 1. Configuration:

Configure user name, password, shell (bash is default), $HOME
(default 0700) and ssh pubkey name in `adduser-ansible/vars/main.yml`

`adduser_password` field should contain shadowed password file. You
can generate this file with the command:
```
cat /dev/urandom | env LC_CTYPE=C tr -cd 'a-f0-9' | head -c 32
```
put generated password into `mkpasswd` to encrypt with `crypt(3)`
libc with salt.
`mkpasswd --method=sha-512 -s`

Use can specify `adduser_expires` in `adduser-ansible/vars/main.yml`
to expire user after specific time. Ask me for desired time to
include a proper variable :)

You can specify `adduser_sudoroot: true` to put user into
`/etc/sudoers` with visudo. Default is `adduser_sudoroot: false`

#### 2. Ansible execution:

Edit inventory file and put all hosts where you'd like to add
specific user. You can organize this file with host groups or use
single hosts. Format is:
$hostname ansible_host=ip_address

Add ssh-public key of user into files directory and specify in
vars/main.yml to whom this key belong to:
```
adduser_public_keys:
  - examplekey.pub
```

To execure ansible role, run:
```
ansible-playbook -i inventory playbook.yml
```
to run it in dry-mode and not execute it remotely run:
```
ansible-playbook -i inventory playbook.yml --check
```
