# Ansible

Ansible is an IT automation tool. It can configure systems, deploy software, and orchestrate more advanced IT tasks such as continuous deployments or zero downtime rolling updates.

When you run Ansible commands or playbooks, Ansible will ssh into the remote machine, copy over the module code (written in Python) and run the module using the arguments specified in the playbook.
The target machine must have a Python interpreter for Ansible to be able to execute these modules and thus configure your machine.

Ansible `raw` and `script` modules do not require python on the remote system and run directly through the remote shell.
This feature can be used to bootstrap a lightweight Python interpreter onto hosts such as CoreOS.


## Install
```
pacman -S ansible
```

## Inventory Setup
The inventory file defines the hosts and groups.
Your public SSH key should be located in authorized_keys on those systems.
```
sudo nano /etc/ansible/hosts
```

By default Ansible assumes it can find a /usr/bin/python on your remote system that is a 2.X version of Python.
You can tell Ansible where to find Python by setting the ansible_python_interpreter variable in your Ansible inventory file.

## Ad-Hoc Commands
```
ansible all -m raw -a "docker ps"
ansible all -m script -a bootstrap.sh
```

## Playbooks
Playbooks are expressed in YAML format and have a minimum of syntax, which intentionally tries to not be a programming language or script, but rather a model of a configuration or a process.
Each playbook is composed of one or more ‘plays’ in a list.
The goal of a play is to map a group of hosts to some well defined roles, represented by things ansible calls tasks. A task is a call to an ansible module.

Run a playbook using a parallelism level of 10:
```
ansible-playbook playbook.yml -f 10
```

To see what hosts would be affected by a playbook before you run it:
```
ansible-playbook playbook.yml --list-hosts
```
