# Ansible

Ansible is a:

* Simple automation language that can perfectly describe an IT application infrastructure in Ansible Playbooks.
* Automation engine that runs Ansible Playbooks.

## macOS installation

```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py --user
python -m pip install --user ansible
python -m pip install --user paramiko
```

[↑ Installing Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

## Components

* **Playbooks** — plain-text YAML files that describe the desired state of something. They are human and machine readable and can be used to build entire application environments.
* **Variables**
* **Inventories** — list of things you want to automate from. In order to run a task or an ansible commmand we need a list of targets on which to automate.
* **Playbooks**. Playbooks contain **plays**. Plays contain **tasks**. Tasks call **modules**. Tasks run sequentially. **Handlers** are griggered by tasks, and are run once, at the end of plays.

## Links

* [↑ How to Install and Configure Ansible on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-18-04)

* [↑ Configuration Management 101: Writing Ansible Playbooks](https://www.digitalocean.com/community/tutorials/configuration-management-101-writing-ansible-playbooks)
