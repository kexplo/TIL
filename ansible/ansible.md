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


## Async

http://docs.ansible.com/ansible/latest/playbooks_async.html

## Serial

## Strategies

reference: http://docs.ansible.com/ansible/latest/playbooks_strategies.html


> `strategy`, by default plays will still run as they used to, with what we call the linear strategy. All hosts will run each task before any host starts the next task, using the number of forks (default 5) to parallelize.
>
> The `serial` directive can ‘batch’ this behaviour to a subset of the hosts, which then run to completion of the play before the next ‘batch’ starts.
>
> A second `strategy` ships with ansible `free`, which allows each host to run until the end of the play as fast as it can.


## Blocks

reference: http://docs.ansible.com/ansible/latest/playbooks_blocks.html

> a block feature to allow for logical grouping of tasks and even in play error handling.

-----


## apt - Manages apt-packages

reference: http://docs.ansible.com/ansible/latest/apt_module.html

```yml
- name Install 'foo' package
  apt:
    name: foo
    state: present
``

## copy - Copies files to remote locations

reference: http://docs.ansible.com/ansible/latest/copy_module.html

| parameter | description |
|-----------|-------------|
| src       | Local path to a file to copy to the remote server; can be absolute or relative. If path is a directory, it is copied recursively. In this case, if path ends with "/", only inside contents of that directory are copied to destination. Otherwise, if it does not end with "/", the directory itself with all contents is copied. This behavior is similar to Rsync. |

```yml
- copy:
    src: /src/file/path
    dest: /dest/file/path
```


## lineinfile - Ensure a particular line is in a file, or replace an existing line using a back-referenced regular expression

reference: http://docs.ansible.com/ansible/latest/lineinfile_module.html


```yml
- lineinfile:
    path: /etc/blahblahblah
    line: '127.0.0.1'

- lineinfile:
    path: /etc/selinux/config
    regexp: '^SELINUX='
    line: 'SELINUX=enforcing'

- lineinfile:
    path: /etc/httpd/conf/httpd.conf
    regexp: '^Listen '
    insertafter: '^#Listen '
    line: 'Listen 8080'
```