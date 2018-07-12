# Ansible

Ansible is an IT automation tool. It can configure systems, deploy software, and orchestrate more advanced IT tasks such as continuous deployments or zero downtime rolling updates.

Ansible aims to be:

- Clear - Ansible uses a simple syntax (YAML) and is easy for anyone (developers, sysadmins, managers) to understand. APIs are simple and sensible.
- Fast - Fast to learn, fast to set up—especially considering you don’t need to install extra agents or daemons on all your servers!
- Complete - Ansible does three things in one, and does them very well. Ansible’s ‘batteries included’ approach means you have everything you need in one complete package.
- Efficient - No extra software on your servers means more resources for your applications. Also, since Ansible modules work via JSON, you can easily extend Ansible with modules in a programming language you already know.
- Secure - Ansible uses SSH, and requires no extra open ports or potentially-vulnerable daemons on your servers.”

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
Machines can be in many groups at one time. Groups are used to allow you to configure many machines at once.
```
sudo nano /etc/ansible/hosts
```

```
[server_group]
140.xxx.xxx.xxx:2222 ansible_user=root
```

By default Ansible assumes it can find a /usr/bin/python on your remote system that is a 2.X version of Python.
You can tell Ansible where to find Python by setting the ansible_python_interpreter variable in your Ansible inventory file.

## Ad-Hoc Commands
```
ansible all -m raw -a "docker ps"
ansible all -m script -a bootstrap.sh
ansible all -m ping  # test connectivity between the controller machine and the managed machine
ansible all -m setup  # gathers facts about the managed machine
ansible machines -m command -a "/bin/echo hello" # run arbitrary command on the managed machine
ansible machines -m shell -a "cd /www/web/ujiamanager;git pull" # use features available in a shell while running your command
ansible machines -a "date" # default module is command
ansible machines -m copy -a "src=/etc/fstab dest=/tmp/fstab" # copy a file on the controller machine to the managed machine
ansible machines -m fetch -a "src=/etc/hosts dest=/tmp” # download file from the managed machine
ansible machines -b -K -m yum -a "name=ntp state=installed"  # use ansible’s yum module to install the NTP daemon
ansible machines -b -K -m service -a "name=ntpd state=started enabled=yes” # make sure NTP is started and set to run on boot
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
