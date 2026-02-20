# Ansible

[↑ **Ansible**](https://docs.ansible.com) is an open source IT automation engine that automates provisioning, configuration management, application deployment, orchestration, and many other IT processes.

## Table of contents

- [Ansible](#ansible)
  - [Table of contents](#table-of-contents)
  - [Components](#components)
  - [Usage](#usage)

## Components

- **Playbooks** — plain-text YAML files that describe the desired state of something. They are human and machine readable and can be used to build entire application environments.
- **Variables**
- **Inventories** — list of things you want to automate from. In order to run a task or an Ansible command we need a list of targets on which to automate.
- **Playbooks**. Playbooks contain **plays**. Plays contain **tasks**. Tasks call **modules**. Tasks run sequentially. **Handlers** are triggered by tasks, and are run once, at the end of plays.

## Usage

1\. If `/etc/ansible/hosts` file doesn't already exist, create it:

```bash
sudo mkdir /etc/ansible
sudo vim /etc/ansible/hosts
```

Example of `hosts` file contents:

```text
[nodes]
node1 ansible_ssh_host=node1.my-host.com ansible_ssh_user=john
node2 ansible_ssh_host=node2.my-host.com ansible_ssh_user=john

[masters]
master1 ansible_ssh_host=178.154.235.126 ansible_ssh_user=john

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

The `all:vars` subgroup sets the `ansible_python_interpreter` host parameter that will be valid for all hosts included in this inventory. This parameter makes sure the remote server uses the `/usr/bin/python3` Python 3 executable instead of `/usr/bin/python` (Python 2.7), which is not present on recent Ubuntu versions.

Whenever you want to check your inventory, you can run:

```bash
ansible-inventory --list -y
```

Make sure that you have [↑ set up your ssh keys](https://stackoverflow.com/questions/2419566/best-way-to-use-multiple-ssh-private-keys-on-one-client) for different hosts propery.

2\. To check that everything works fine run Ansible's `ping` module:

```bash
ansible -m ping nodes
```

3\. Run [povisioning.yml](provisioning.yaml) playbook:

```bash
ansible-playbook -l nodes provisioning.yaml
```

`provisioning.yaml` file:

```yaml
- hosts: all
  become: true
  become_user: root
  tasks:
    - name: Disable welcome message after SSH login
      file:
        path: /etc/update-motd.d
        state: directory
        recurse: yes
        mode: -x

    - name: Set timezone to Europe/Moscow
      timezone:
        name: Europe/Moscow

    - name: Set up "cls" alias
      become: false
      blockinfile:
        path: ~/.bashrc
        block: alias cls='clear'

    - name: Update cache and install "htop" package
      apt:
        name: htop
        update_cache: yes

    - name: Update cache and upgrade all apt packages
      apt:
        upgrade: 'yes'
        update_cache: yes

    - name: Unconditionally reboot the machine with all defaults
      reboot:
      msg: "Reboot initiated by Ansible"
        connect_timeout: 5
        reboot_timeout: 600
        pre_reboot_delay: 0
        post_reboot_delay: 30
        test_command: whoami
```
