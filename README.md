# Jenkins PHP Ansible

A set of playbooks for setting up a Jenkins server and LAMP hosts for a development servers.

This will not work out of the box, some setup is required.

Copy `var/main-vars.sample.yml` to `var/main-vars.yml` and updated the varaibles for your setup.

## Commands

### First run

```shell
ansible-galaxy install -r requirements.yml -f
ansible-playbook setup.yml
ansible-playbook main.yml
```

### 2-N runs

```shell
ansible-playbook main.yml
```

### Adding a new webserver; i.e. web30

```shell
ansible-playbook setup.yml --limit=web03
ansible-playbook main.yml
```

## Local ssh config

Setup your SSH config file to include the servers you are working with `~/.ssh/config`.


```
Host jenkins
    Hostname remoteip # Remote IP Address
    User demo # `user_sudouser_username` from var/main-vars.yml
    Port 7822 # `remote_port` from var/main-vars.yml

Host web01
    Hostname remoteip
    User demo
    Port 7822

Host web02
    Hostname remoteip
    User demo
    Port 7822
```

## Ansible Hosts file

Edit your hosts file to have a `jenkinsgroup`, `webgroup` and `demogroup`, if you use different names for the groups you can do a find and replace in the files.

To find the path for the invitory file:

```shell
ansible --version
ansible 2.6.3
  config file = /Users/username/.ansible.cfg
  configured module search path = [u'/Users/username/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/Cellar/ansible/2.6.3/libexec/lib/python2.7/site-packages/ansible
  executable location = /usr/local/bin/ansible
  python version = 2.7.15 (default, Jul 23 2018, 21:27:06) [GCC 4.2.1 Compatible Apple LLVM 9.1.0 (clang-902.0.39.2)]

grep --color=auto -iR 'inventory' ~/.ansible.cfg
/Users/username/.ansible.cfg:#inventory      = /etc/ansible/hosts
/Users/username/.ansible.cfg:inventory = ~/.ansible/hosts
```

```ini
[jenkinsgroup] 
jenkins

[webgroup] 
web01 php_version='7.1'
web02 php_version='7.1'

[demogroup:children]
jenkinsgroup
webgroup
```

If your `jenkins` host is has anohter name, rename `var/jenkins.yml` to match. i.e. `jenkins-dev` `var/jenkins-dev.yml`

```ini
[jenkinsgroup] 
jenkins-dev
```

