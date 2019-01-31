---
description: Ansible Cheatsheet
---

# Ansible

Install ansible linter to check the file is ok or not:

```text
sudo apt install ansible-lint
```

then:

```text
ansible-lint playbook.yaml
```

Check if the server is running:

```bash
ansible all -i hostname, -m ping
```

Expected output:

```bash
hostname | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

Run command on host:

```bash
ansible all -i hostname, -m shell -a 'echo hello world'
```

Playbook file should name something like `FILENAME.yaml`.

it's better to describe the name of the file base on the task of that file like `apache.yaml`.

{% code-tabs %}
{% code-tabs-item title="apache.yaml" %}
```yaml
---
- hosts: all
  remote_user: root
  tasks:
  - name: Install Apache2 on server
    yum:
      name: httpd
      state: present
  - name: Ensure that apache2 is running
    service:
      name: httpd
      state: started
      enable: True
    become: True
```
{% endcode-tabs-item %}
{% endcode-tabs %}


Run the playbook:

```bash
ansible-playbook -i hostname, apache.yaml
```

Show all configs and variables of a host:

```bash
ansible all -i hostname, -m setup
```

Using Ansible setup variable:

{% code-tabs %}
{% code-tabs-item title="ansible\_var.yaml" %}
```yaml
---
- hosts: all
  remote_user: root
  tasks:
  - name: debugging from setup 
    debug:
      msg: "OS version is '{{ ansible_distribution }}' '{{ ansible_distribution_file_variety }}' with IP: '{{ ansible_all_ipv4_addresses }}'"
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Output:

```bash
TASK [debugging from setup] ********************************************************************************************************************************************
ok: [centosvm] => {
    "msg": "OS version is 'CentOS' 'RedHat' with IP: '['192.168.43.111', '192.168.43.64']'"
}

```

Inline var:

{% code-tabs %}
{% code-tabs-item title="var1.yaml" %}
```yaml
---
- hosts: all
  remote_user: root
  tasks:
  - name: Variable assigning
    set_fact:
        pkg1: sqlite
  - name: Install Apache2 on server
    yum:
      name: httpd
      name: "{{ pkg1 }}"
      state: present
  - name: Ensure that apache2 is running
    service:
      name: httpd
      state: started
    become: True
```
{% endcode-tabs-item %}
{% endcode-tabs %}

then run the playbook.

Another way:

{% code-tabs %}
{% code-tabs-item title="var2.yaml" %}
```yaml
---
- hosts: all
  remote_user: root
  tasks:
  - name: Install Apache2 on se
rver
    yum:
      name: httpd
      name: "{{ pkg1 }}"
      state: present
  - name: Ensure that apache2 is running
    service:
      name: httpd
      state: started
    become: True
```
{% endcode-tabs-item %}
{% endcode-tabs %}

and execute this:

```bash
 ansible-playbook -i hostname, var.yaml -e 'pkg1=vim'
```

on CentOS, enable the EPEL

{% code-tabs %}
{% code-tabs-item title="epel.yaml" %}
```yaml
---
- hosts: all
  remote_user: root
  tasks:
  - name: Install epel
    yum:
      name: epel-release
      state: present
```
{% endcode-tabs-item %}
{% endcode-tabs %}

upgrade a package

{% code-tabs %}
{% code-tabs-item title="apache\_upgrade.yaml" %}
```yaml
---
- hosts: all
  remote_user: root
  tasks:
  - name: Install Apache2 on s
erver
    yum:
      name: httpd
      state: latest
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Or upgrade all:

{% code-tabs %}
{% code-tabs-item title="upgrade.yaml" %}
```yaml
---
- hosts: all
  remote_user: root
  tasks:
  - name: Upgrade all packages
    yum:
      name: "*"
      state: Latest
```
{% endcode-tabs-item %}
{% endcode-tabs %}

to list all tasks:

```text
ansible-playbook taskname.yaml --list-tasks 

```

Generating static files using jinja2 and ansible

{% code-tabs %}
{% code-tabs-item title="jinja.yaml" %}

```yaml
---
- hosts: all
  remote_user: root
  tasks:
  - name: testing jinja2
    template:
      src: index.html.j2
      dest: /var/www/html/index.html
      owner: root
      group: root
      mode: 0644

```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="index.html.j2" %}
```markup
<html>
<body>
<h1>Hello World!</h1>
<p>This page was created on {{ ansible_date_time.date
}}.</p>
{% if ansible_eth0.active == True %}
<p>eth0 address {{ ansible_eth0.ipv4.address }}.</p>
{% endif %}
</body>
</html>

```
{% endcode-tabs-item %}
{% endcode-tabs %}

[More Doc for Jinja2](https://jinja.pocoo.org/docs/2.10/)

Working with Hosts

{% code-tabs %}
{% code-tabs-item title="hostname.txt" %}
```text
[general]
192.168.1.100
ex.sample.com
ex[1:2].sample.com
db.sample.com

[webserver]
192.168.1.100
ex2.sample.com

[database]
db.sample.com

```
{% endcode-tabs-item %}
{% endcode-tabs %}

Sample of a playbook which only run tasks on webserver group

{% code-tabs %}
{% code-tabs-item title="apache.yaml" %}
```yaml
---
- hosts: webserver
  remote_user: root
  tasks:
  - name: Install Apache2 on server
    yum:
      name: httpd
      state: present
  - name: Ensure that apache2 is running
    service:
      name: httpd
      state: started
      enable: True
    become: True
```
{% endcode-tabs-item %}
{% endcode-tabs %}

for running:

```text
ansible-playbook -i hostname.txt, apache2.yaml

```

In this section, I decided to write more about variables. but i think i should refer you to the [Official Documents](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html).

please note that you should also read the documents of ansible for each modules's variable for better performance. 

for working with iteration use `with_items`

{% code-tabs %}
{% code-tabs-item title="general.yaml" %}
```yaml
---
- hosts: all_machines
  remote_user: root
  tasks:
  - name: install nececery tools
    yum:
      name: '{{ item }}'
      state: present
    with_items:
    - vim
    - sqlite
    - openjdk-jre
    - docker 

```
{% endcode-tabs-item %}
{% endcode-tabs %}

other method. make a backup of a file/directory

{% code-tabs %}
{% code-tabs-item title="general.yaml" %}
```yaml
---
- hosts: all_machines
  remote_user: root
  vars:
    dirs:
    - /etc/named.conf
    - /var/www/html/index.html
    users:
    - numb
  tasks:
  - name: ensure user is exist
    users:
      name: '{{item}}'
    become: yes
    with_items:
    - '{{ users }}'
    - name: ensure user directories exist
     file:
       path: '/home/{{ item }}'
       state: directory
     become: yes
     with_items:
     - '{{ users }}'
     - name: copy / backup files from src to dest
     copy: 
       src: '{{ item.0 }}'
       dest: '/home/{{ item.1 }}'
     become: yes
     with_nested:
     - '{{ dirs }}'
     - '{{ users }}'
       
```
{% endcode-tabs-item %}
{% endcode-tabs %}

working with sequences:

{% code-tabs %}
{% code-tabs-item title="sequence.yaml" %}
```yaml
---
- hosts: webservers
  remote_user: root
  tasks: 
  - name: create some file in sequence
    file:
      dest: '/home/backup{{ item }}'
      state: directory
    with_sequence: start=1 end=4
    become: yes
```
{% endcode-tabs-item %}
{% endcode-tabs %}

you can also change `hostname` with sequences

condition could be look like this:

{% code-tabs %}
{% code-tabs-item title="apache.yaml" %}
```yaml
---
- hosts: webserver
  remote_user: root
  tasks:
  - name: Install Apache2 on Redhat based
    yum:
      name: httpd
      state: present
    when: ansible_os_family =='RedHat'
  - name: Install Apache2 on debian based
    apt:
      name: apache2
      state: present
    when: ansible_os_family == 'Debian'
  - name: Ensure that apache2 is running
    service:
      name: httpd
      state: started
      enable: True
    become: True
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Boolean  condition

```text
---
- hosts: all
  remote_user: ansible
  vars:
   backup: True
  tasks:
  - name: Copy the crontab in tmp if the backup variable is true
  copy:
    src: /etc/crontab
    dest: /tmp/crontab
    remote_src: True
  when: backup
```

simply create ansible project structure with this:

```bash
mkdir -p roles/common/{tasks,handlers,templates,files,vars,defaults,meta}
touch roles/common/{tasks,handlers,templates,files,vars,defaults,meta}/main.yml
```

secret vault

inventory example

\[servers:vars\]

```yaml
ansible_user=numb
ansible_become=yes
ansible_become_method=sudo 
ansible_become_pass='{{ server_pass }}'
```

create a new encripted secret file

`ansible-vault create passwords.yml`

after adding password for file add this to the opened file

```
server_pass: PASSWORD
```

then run the ansible

`ansible-playbook -i inventory --ask-vault-pass --extra-vars '@passwords.yml' ansible.yml`

TODO

write about roles structure, templates, groups, helpers, bash module, docker module, python module, diff, optimizing.
