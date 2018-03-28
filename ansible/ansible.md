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

doc: http://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html

Ansible works against multiple systems in your infrastructure at the same time. It does this by selecting portions of systems listed in Ansible’s inventory, which defaults to being saved in the location `/etc/ansible/hosts`. You can specify a different inventory file using the `-i <path>` option on the command line.

### Host and Groups

INI format:

```ini
mail.example.com

[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
three.example.com
```

YAML format:

```yml
all:
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
        foo.example.com:
        bar.example.com:
    dbservers:
      hosts:
        one.example.com:
        two.example.com:
        three.example.com:
```

To make things explicit, it is suggested that you set them if things are not running on the default port:

```ini
jumper ansible_port=5555 ansible_host=192.0.2.50
```

```yml
...
  hosts:
    jumper:
      ansible_port: 5555
      ansible_host: 192.0.2.50
```

### Patterns

```ini
[webservers]
www[01:50].example.com
```

```ini
[databases]
db-[a:f].example.com
```

### Group Variable

```ini
[atlanta]
host1
host2

[atlanta:vars]
ntp_server=ntp.atlanta.example.com
proxy=proxy.atlanta.example.com
```

```yml
atlanta:
  hosts:
    host1:
    host2:
  vars:
    ntp_server: ntp.atlanta.example.com
    proxy: proxy.atlanta.example.com
```


### Groups of Groups

```ini
[atlanta]
host1
host2

[raleigh]
host2
host3

[southeast:children]
atlanta
raleigh

[southeast:vars]
some_server=foo.southeast.example.com
halon_system_timeout=30
self_destruct_countdown=60
escape_pods=2

[usa:children]
southeast
northeast
southwest
northwest
```

```yml
all:
  children:
    usa:
      children:
        southeast:
          children:
            atlanta:
              hosts:
                host1:
                host2:
            raleigh:
              hosts:
                host2:
                host3:
          vars:
            some_server: foo.southeast.example.com
            halon_system_timeout: 30
            self_destruct_countdown: 60
            escape_pods: 2
        northeast:
        northwest:
        southwest:
```

### Default Groups

- `all` : contains every host.
- `ungrouped` : contains all hosts that don't have another group aside from `all`

## Dynamic Inventory



## Ad-hoc command

doc: http://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html


## Playbook

doc: http://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html

> Playbooks are a completely different way to use ansible than in adhoc task execution mode, and are particularly powerful.

example:

```yml
---
- hosts: webservers
  vars:
    http_port: 80
    max_clients: 200
  remote_user: root
  tasks:
  - name: ensure apache is at the latest version
    yum:
      name: httpd
      state: latest
  - name: write the apache config file
    template:
      src: /srv/httpd.j2
      dest: /etc/httpd.conf
    notify:
    - restart apache
  - name: ensure apache is running (and enable it at boot)
    service:
      name: httpd
      state: started
      enabled: yes
  handlers:
    - name: restart apache
      service:
        name: httpd
        state: restarted
```


### Handlers: Running Operations on Change

```yml
- name: template configuration file
  template:
    src: template.j2
    dest: /etc/foo.conf
  notify:
     - restart memcached
     - restart apache
```

```yml
handlers:
    - name: restart memcached
      service:
        name: memcached
        state: restarted
    - name: restart apache
      service:
        name: apache
        state: restarted
```

As of Ansible 2.2, handlers can also “listen” to generic topics, and tasks can notify those topics as follows:

```yml
handlers:
    - name: restart memcached
      service:
        name: memcached
        state: restarted
      listen: "restart web services"
    - name: restart apache
      service:
        name: apache
        state:restarted
      listen: "restart web services"

tasks:
    - name: restart everything
      command: echo "this task will restart the web services"
      notify: "restart web services"
```

### Loop

```yml
- name: add several users
  user:
    name: "{{ item }}"
    state: present
    groups: "wheel"
  loop:
     - testuser1
     - testuser2
```


before 2.5:

```yml
- name: add several users
  user:
    name: "{{ item }}"
    state: present
    groups: "wheel"
  with_items:
     - testuser1
     - testuser2
```


### Filter

Filters in Ansible are from Jinja2, and are used for transforming data inside a template expression. Jinja2 ships with many filters. See [builtin filters](http://jinja.pocoo.org/docs/templates/#builtin-filters) in the official Jinja2 template documentation.

```yml
{{ some_variable | to_json }}
{{ some_variable | to_yaml }}
```

```yml
tasks:
  - shell: cat /some/path/to/file.json
    register: result

  - set_fact:
      myvar: "{{ result.stdout | from_json }}"
```


### Conditionals

```yml
tasks:
  - name: "shut down Debian flavored systems"
    command: /sbin/shutdown -t now
    when: ansible_os_family == "Debian"
    # note that Ansible facts and vars like ansible_os_family can be used
    # directly in conditionals without double curly braces
```

### Syntax check

To check the syntax of a playbook, use `ansible-playbook` with the `--syntax-check` flag. This will run the playbook file through the parser to ensure its included files, roles, etc. have no syntax problems.


## Role

doc: http://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html

Example project structure:

```
site.yml
webservers.yml
fooservers.yml
roles/
   common/
     tasks/
     handlers/
     files/
     templates/
     vars/
     defaults/
     meta/
   webservers/
     tasks/
     defaults/
     meta/
```

- `tasks` - contains the main list of tasks to be executed by the role.
- `handlers` - contains handlers, which may be used by this role or even anywhere outside this role.
- `defaults` - default variables for the role (see Variables for more information).
- `vars` - other variables for the role (see Variables for more information).
- `files` - contains files which can be deployed via this role.
- `templates` - contains templates which can be deployed via this role.
- `meta` - defines some meta data for this role. See below for more details.


### Example

```yml
# roles/example/tasks/main.yml
- name: added in 2.4, previously you used 'include'
  import_tasks: redhat.yml
  when: ansible_os_platform|lower == 'redhat'
- import_tasks: debian.yml
  when: ansible_os_platform|lower == 'debian'

# roles/example/tasks/redhat.yml
- yum:
    name: "httpd"
    state: present

# roles/example/tasks/debian.yml
- apt:
    name: "apache2"
    state: present
```

Using role:

```yml
---
- hosts: webservers
  roles:
     - common
     - webservers
```


## Ansible-Pull

The ansible-pull is a small script that will checkout a repo of configuration instructions from git, and then run ansible-playbook against that content.


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


## Modules

http://docs.ansible.com/ansible/latest/modules/list_of_all_modules.html

### apt - Manages apt-packages

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


### lineinfile - Ensure a particular line is in a file, or replace an existing line using a back-referenced regular expression

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