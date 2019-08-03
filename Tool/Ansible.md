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
ansible -i inventory-file host-pattern -m ping
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

list all tasks that would be executed

```
ansible-playbook playbook.yml --list-tasks
```

set additional variables as key=value

```
ansible-playbook playbook.yml  -e key=value
```

To run an entire playbook locally, just set the “hosts:” line to “hosts: 127.0.0.1” and then:

```
ansible-playbook playbook.yml --connection=local
```

Transfer files between two remote machines

```yml
- hosts: ServerB
  tasks:
    - name: Transfer file from ServerA to ServerB
      synchronize:
        src: /path/on/server_a
        dest: /path/on/server_b
      delegate_to: ServerA
```

Or use pull mode to invert the transfer direction.

```yml
- hosts: ServerA
  tasks:
    - name: Transfer file from ServerA to ServerB
      synchronize:
        src: /path/on/server_a
        dest: /path/on/server_b
        mode: pull
      delegate_to: ServerB
```

## [AWX](https://github.com/ansible/awx)

安装

```
git clone -b 4.0.0 https://github.com/ansible/awx.git
cd awx/install
nano inventory
  postgres_data_dir=/home/junfeng/pgdocker
  docker_compose_dir=/home/junfeng/awxcompose
  host_port=8052
ansible-playbook -i inventory install.yml
docker logs -f awx_task
```

升级

```
docker-compose stop
nano docker-compose.yml
docker-compose pull && docker-compose up --force-recreate -d
```

AWX 相比于 Jenkins 的优点：
1、使用 Ansible Playbook 编写部署脚本，所有脚本存在 Git 仓库中，便于多人协作完善，更改记录可追溯。
2、同一个 Playbook 使用不同的参数可创建多个 Job 模板，减少了维护大量 Job 脚本的工作量。
3、同一个 Job 实例可同时部署多个服务器，能应对大规模的服务器集群。
4、服务器的登录密码与部署脚本是分离存储的，安全性更好。
5、服务器清单有明确的定义。
6、网页界面更美观。
