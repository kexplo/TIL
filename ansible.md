# Ansible

document: http://docs.ansible.com/ansible/latest/intro_getting_started.html

## Installation

Latest releases (Debian):

```bash
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt-get update
$ sudo apt-get install ansible
```

pip:

```bash
$ pip install ansible
```

## Remote Connection Information

> By default, Ansible 1.3 and later will try to use native OpenSSH for remote communication when possible. This enables ControlPersist (a performance feature), Kerberos, and options in `~/.ssh/config` such as Jump Host setup. However, when using Enterprise Linux 6 operating systems as the control machine (Red Hat Enterprise Linux and derivatives such as CentOS), the version of OpenSSH may be too old to support ControlPersist. On these operating systems, Ansible will fallback into using a high-quality Python implementation of OpenSSH called ‘paramiko’. If you wish to use features like Kerberized SSH and more, consider using Fedora, OS X, or Ubuntu as your control machine until a newer version of OpenSSH is available for your platform – or engage ‘accelerated mode’ in Ansible. See [Accelerated Mode.](http://docs.ansible.com/ansible/latest/playbooks_acceleration.html)


## Inventory

## Dynamic Inventory

## Ad-hoc command