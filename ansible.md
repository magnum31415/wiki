
# Ansible Cheat Sheet
[Ansible Cheat Sheet](https://github.com/magnum31415/wiki/blob/main/Ansible_Cheat_Sheet.pdf)

# Instalar venv ansible

Installing the core Ansible toolset involves relatively few steps and has minimal requirements. On the other hand, installing the additional components that Red Hat Ansible Automation Platform provides, such as the automation controller (formerly called Red Hat Ansible Tower), requires a Red Hat Enterprise Linux 8.2 or later system, with a minimum of two CPUs, 4 GiB of RAM, and 20 GiB of available disk space. 

• You need a valid Red Hat Ansible Automation Platform subscription to install the core toolset on your control node. 
````
subscription-manager register 
subscription-manager repos --enable ansible-2-for-rhel-8-x86_64-rpms 
yum install ansible 
ansible --version 
ansible -m setup localhost | grep ansible_python_version

````


## Instalar en un venv
```
python3 -m venv ansible_2.17
source ansible_2.17/bin/activate
pip install ansible-core==2.17
pip install pywinrm
```


| Ansible Version | Python 2.6 | Python 2.7 | Python 3.5 | Python 3.6 | Python 3.7 | Python 3.8 | Python 3.9 | Python 3.10 |
|-----------------|------------|------------|------------|------------|------------|------------|------------|-------------|
| 2.9.x           | ✓          | ✓          | ✓          | ✓          | ✓          | ✗          | ✗          | ✗           |
| 2.10.x          | ✗          | ✓          | ✓          | ✓          | ✓          | ✓          | ✗          | ✗           |
| 2.11.x          | ✗          | ✓          | ✓          | ✓          | ✓          | ✓          | ✓          | ✗           |
| 2.12.x          | ✗          | ✗          | ✗          | ✓          | ✓          | ✓          | ✓          | ✓           |
| 2.13.x          | ✗          | ✗          | ✗          | ✓          | ✓          | ✓          | ✓          | ✓           |
| 2.14.x          | ✗          | ✗          | ✗          | ✓          | ✓          | ✓          | ✓          | ✓           |
| 2.15.x          | ✗          | ✗          | ✗          | ✓          | ✓          | ✓          | ✓          | ✓           |




## Proxychains

```
sudo apt-get install proxychains
vim /etc/proxychains.conf 

Add in [ProxyList]
socks5 12.34.11.123 1080
```
# Molecule

````
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
source myenv/bin/activate
pip install molecule molecule-docker yamllint pytest testinfra
molecule drivers
ansible-galaxy init my_role
cd my_role

molecule init scenario --driver-name docker



 molecule login --host geerlingguy-centos8
````

# Vars
Variables can be defined in a variety of places in an Ansible project 
You can set a variable that affects a group of hosts or only individual hosts.
Some variables are facts that can be set by Ansible based on the configuration of a system. 
Other variables can be set inside the playbook, and affect one play in that playbook, or only one task in that play. 
You can also set extra variables on the ansible-playbook command line by using the --extra-vars or -e option and specifying those variables, and they override all other values for that variable name. 
 
List of ways to define a variable, ordered from lowest precedence to highest: 

1. Group variables defined in the inventory. 
2. Group variables defined in files in a group_vars subdirectory in the same directory as the inventory or the playbook. 
3. Host variables defined in the inventory. 
4. Host variables defined in files in a host_vars subdirectory in the same directory as the inventory or the playbook.
5. Host facts, discovered at runtime.
6. Play variables in the playbook (vars and vars_files).
7. Task variables.
8. Extra variables defined on the command line. 

If the same variable name is defined at more than one level, the level with the highest precedence wins. A narrow scope, such as a host variable or a task variable, takes precedence over a wider scope, such as a group variable or a play variable. Variables defined by the inventory are overridden by variables defined by the playbook.

Variables are referenced by placing the variable name in double curly braces ({{ }}). Ansible substitutes the variable with its value when the task is executed. 
```
vars: 
user: joe 
tasks: 
  # This line will read: Creates the user joe 
  - name: Creates the user {{ user }} 
    user:   
      # This line will create the user named Joe 
      name: "{{ user }}" ![image](https://github.com/user-attachments/assets/cb9dea2c-6879-43e6-8852-ae0d5bc7c492)
```


* vars block **at the beginning** of a playbook:
```
- hosts: all
  vars: 
    user: joe 
    home: /home/joe
```  

* define playbook variables **in external files**
```
- hosts: all 
  vars_files: 
    - vars/users.yml ![image](https://github.com/user-attachments/assets/4f9fe7ce-bef5-4c76-93c7-94858bfb9333)
```

* The **preferred approach** to defining variables for hosts and host groups is to create two directories, group_vars and host_vars, in the same working directory as the inventory file or playbook.
  * To define group variables for the servers group, you would create a YAML file named group_vars/servers, and then the contents of that file would set variables to values using the same syntax as in a playbook: user: joe 
  * To define host variables for a particular host, create a file with a name matching the host in the host_vars directory to contain the host variables.

* **Extra variables** can be useful when you need to override the defined value for a variable for a one-off run of a playbook. For example: 
```` ansible-playbook main.yml -e "package=apache" ````

# Commandos 
```
proxychains ansible-playbook -i ../inventario.ini -l SERVERNAMEO01 playbook.yml --vault-password-file ../vault_pass.txt
```

```
ansible -i ../inventario.ini server01 -m setup -a 'filter=ansible_fqdn' 
ansible -i ../inventario.ini server01 -m setup -a 'filter=ansible_hostname' 
```

```
ansible '*.example.com, !*.lab.example.com'  -i inventory1 --list-hosts 
ansible  lb1.lab.example.com,s1.lab.example.com,db1.example.com -i inventory1  --list-hosts 
ansible '172.25.*' -i inventory1 --list-hosts 
ansible 's*' -i inventory1 --list-hosts 
ansible 'prod,172*,*lab*' -i inventory1  --list-hosts 
ansible 'db,&london' -i inventory1  --list-hosts 
```

# ansible.cfg

ANSIBLE_CONFIG environment variable overrides all other configuration files. If that variable is not set, the directory in which the ansible command was run is then checked for an ansible.cfg file. If that file is not present, the user's home directory is checked for a .ansible.cfg file. The global /etc/ansible/ansible.cfg file is only used if no other configuration file is found. If the /etc/ansible/ansible.cfg configuration file is not present, Ansible contains defaults which it uses. 
 
 to clearly identify which version of Ansible is installed, and which configuration file is being used. 
 ```
[user@controlnode ~]$ ansible --version 
ansible 2.9.21 
config file = /etc/ansible/ansible.cfg 
 
[user@controlnode ~]$ ansible servers --list-hosts -v 
Using /etc/ansible/ansible.cfg as config file 
```

ansible.cfg
````
[defaults] 
inventory=./inventory 
remote_user=curtis 
ask_pass = false 
roles_path = /home/curtis/ansible/roles:/usr/share/ansible/roles 
forks = 8 
log_path = /var/log/ansible/execution.log 
host_key_checking = false 
 
[privilege_escalation] 
become=True 
become_method=sudo 
become_user=root 
become_ask_pass=False

````

##If you want to find out what options are available in the configuration file, use the 
 ````
 ansible-config list 
 ````
##you might want to find out which options have been set to values which are different from the built-in defaults. 
````
ansible-config dump -v --only-changed ![image](https://github.com/user-attachments/assets/eba4aa69-d6d1-411f-b217-6aca8119995a)
````

# ansible-doc

-  the -s option, which produces example output that can serve as a model for how to use a particular module in a playbook.
-  to see a list of the modules available on a control node, run the ansible-doc -l

# Inventory

An inventory defines a collection of hosts that Ansible will manage. These hosts can also be assigned to groups, which can be managed collectively. Groups can contain child groups, and hosts can be members of multiple groups. The inventory can also set variables that apply to the hosts and groups that it defines. 
Host inventories can be defined in two different ways. A static host inventory can be defined by a text file. A dynamic host inventory can be generated by a script or other program as needed, using external information providers. 

Ansible host inventories can include groups of host groups. This is accomplished by creating a host group name with the :children suffix.  

````
[usa] 
washington[1:2].example.com 
 
[canada] 
ontario01.example.com 
ontario02.example.com 
 
[north-america:children] 
canada 
usa 

````
## JumpHost Config

User to execute playbooks /home/user-exe-playbooks/.ssh/config

````
 config
Host *
    Port 22
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null
    ServerAliveInterval 60
    ServerAliveCountMax 30

Host jumpserver
    HostName bastion.company.intern
    User ansible
    IdentityFile ~/.ssh/ansible-jump-host
````

Inventory

````
[all:vars]
ansible_ssh_common_args='-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ProxyCommand="ssh -W %h:%p jumpserver"'
````


## Verifying the Inventory 
ansible washington1.example.com --list-hosts 
ansible usa --list-hosts 
ansible all --list-hosts 
ansible ungrouped --list-hosts 
ansible host-or-group -i inventory --list-hosts

## Special groups

 - **all** that matches all managed hosts in the inventory.
````
 - hosts: all 
````
- **ungrouped**, which includes all managed hosts in the inventory that are not members of any other group: 
 - hosts: ungrouped 
````
- Another method of accomplishing the same thing as the all host pattern is to use the asterisk (*)
 - hosts: * 
````
- (&) matches machines in the lab group only if they are also in the datacenter1 group: 
````
- hosts: lab,&datacenter1
````
- Exclude hosts (!). Ex. all hosts defined in the datacenter group, except test2.example.com  
````
- hosts: datacenter,!test2.example.com
````


# Facts
 
````
ansible webservers -m setup 
````
FACTS filtrat x subset && fine-grained 
````
ansible webservers -m setup -a 'gather_subset=network filter=ansible_interfaces' 
````
several values 
```` 
ansible localhost -m setup -a 'filter=fqdn,hostname,*ipv*' 
````

Use the -o option to display the output of Ansible ad hoc commands in a single line format. 
````
ansible mymanagedhosts -m command -a /usr/bin/hostname -o 
````

#COMMANDS/MODULES 

## ansible-doc
The ansible-doc -l command lists all modules installed on a system. You can use ansible-doc to view the documentation of particular modules by name, and find information about what arguments the modules take as options. 
 ````
Ansible-doc ping 
 ````
The ansible-doc command also offers the -s option, which produces example output that can serve as a model for how to use a particular module in a playbook. 

## ad-hoc

````
ansible dev -m copy  -a 'content="Managed by Ansible\n" dest=/etc/motd' -b -u devops 
ansible localhost -m copy -a 'content="Managed by Ansible\n" dest=/etc/motd' -u devops --become 
ansible all -m copy  -a 'content="Managed by Ansible\n" dest=/etc/motd' -u devops --become 
 
 
 
## verify that the contents of the file 
ansible dev -m command -a "cat /etc/motd" 
ansible all -m command -a 'cat /etc/motd' -u devops 
 
ansible localhost -m command -a 'id' -u devops 
ansible -m user -a "name=newbie uid=4000 state=present"  servera.lab.example.com 
ansible servera.lab.example.com -m command  -a 'systemctl status httpd' 
ansible database_prod -m command  -a 'cat /etc/redhat-release' -u devops --become 
ansible all -m command -a 'ls -Z' -u devops 
ansible webservers -a  'systemctl is-active httpd' 
ansible webservers -a  'systemctl is-enabled httpd' 
ansible webservers -a  'cat /etc/httpd/conf.d/vhost.conf' 
ansible webservers -m uri  -a 'url=http://localhost return_content=true' 
ansible demohost -m ping 
ansible demohost -m ping --become 
ansible demohost -m command -a 'free -m' 
ansible servera.lab.example.com  -u devops -b -a "firewall-cmd --list-services" 
ansible all -m yum  -a 'name=example-motd state=absent' 
ansible dev -m copy  -a 'content="Managed by Ansible\n" dest=/etc/motd' -b -u devops 
ansible localhost -m copy  -a 'content="Managed by Ansible\n" dest=/etc/motd' -u devops --become 
````

## Galaxy

##Using ansible-galaxy, create the directory structure for the new ansible-vsftpd role 
ansible-galaxy init ansible-vsftpd 

# Secrets

````
ansible-playbook --syntax-check  --ask-vault-pass create_users.yml 
ansible-playbook --vault-password-file=vault-pass create_users.yml 

````

# Playbooks

## Verify Syntax
ansible-playbook --syntax-check playbook.yml 

## Dry-run
You can use the -C option to perform a dry run --check

ansible -m user -a "name=newbie uid=4000 state=present"  servera.lab.example.com 



--- 
- name: Configure important user consistently 
hosts: servera.lab.example.com 
tasks: 
- name: newbie exists with UID 4000 
user: 
name: newbie 
uid: 4000 
state: present 



# Estructuras de datos

## Lista

Una lista en Ansible es una colección ordenada de elementos. 
Definición de una lista en Ansible
````
my_list:
  - "elemento1"
  - "elemento2"
  - "elemento3"
````
Lists also have an inline format enclosed in square braces, as follows: 
hosts: [servera, serverb, serverc]

## Dictionary
Un diccionario en Ansible es una colección de pares clave-valor. Puedes definirlo así:
````
---
- name: Ejemplo de acceso a valores en un diccionario de servidores
  hosts: localhost
  gather_facts: no
  vars:
    servers:
      SERVER1:
        entidad: A
        entorno: PRO
        source_host: SERVEROLD1
        grupo: "Web"
      SERVER2:
        entidad: B
        entorno: PRO
        source_host: SERVEROLD2
        grupo: "DB"

  tasks:
    - name: Mostrar la información de SERVER1
      debug:
        msg: "SERVER1 pertenece a la entidad {{ servers.SERVER1.entidad }}, en el entorno {{ servers.SERVER1.entorno }}, con source_host {{ servers.SERVER1.source_host }} y grupo {{ servers.SERVER1.grupo }}"

    - name: Mostrar la información de SERVER2
      debug:
        msg: "SERVER2 pertenece a la entidad {{ servers.SERVER2.entidad }}, en el entorno {{ servers.SERVER2.entorno }}, con source_host {{ servers.SERVER2.source_host }} y grupo {{ servers.SERVER2.grupo }}"

````
En jinja

````
# plantilla.j2
Server: {{ server_name }}
Entidad: {{ servers[server_name].entidad }}
Entorno: {{ servers[server_name].entorno }}
Source Host: {{ servers[server_name].source_host }}
Grupo: {{ servers[server_name].grupo }}
````


## Lista de Diccionarios
````
my_list_of_dicts:
  - nombre: "Alice"
    edad: 28
    ciudad: "Paris"
  - nombre: "Bob"
    edad: 32
    ciudad: "Berlin"

- name: Ejemplo de lista de diccionarios
  hosts: localhost
  tasks:
    - name: Mostrar cada persona en la lista
      debug:
        msg: "Nombre: {{ item.nombre }}, Edad: {{ item.edad }}, Ciudad: {{ item.ciudad }}"
      loop: "{{ my_list_of_dicts }}"

````



# Templates Jinja
A Jinja2 template is composed of multiple elements: data, variables, and expressions

- use {% EXPR %} for expressions or logic (for example, loops)
- user {{ EXPR }} are used for outputting the results of an expression or a variable to the end user.!
- Use {# COMMENT #} syntax to enclose comments

````
{# /etc/hosts line #} 
{{ ansible_facts[default_ipv4][address] }}    {{ ansible_facts[hostname]}}
````

To avoid having system administrators modify files deployed by Ansible.
Ansible managed" string set in the ansible_managed directive in ansible.cfg.

````

#{{ ansible_managed }} 
Port {{ ssh_port }} 
ListenAddress {{ ansible_facts[default_ipv4][address] }} 
HostKey /etc/ssh/ssh_host_rsa_key 
HostKey /etc/ssh/ssh_host_ecdsa_key 
````

## Variable filters

change the output format for template expressions

````
{{ output | to_json }} 
{{ output | to_yaml }}
````

## Loops

````
{% for user in users %} 
{{ user }} 
{% endfor %} 

 
{# for statement #} 
{% for myuser in users if not myuser == "root" %} 
User number {{ loop.index }} - {{ myuser }} 
{% endfor %} 
````

The loop.index variable expands to the index number that the loop is currently on.

````
{% for myhost in groups['myhosts'] %} 
{{ myhost }} 
{% endfor %} 


{% for host in groups['all'] %} 
{{ hostvars[host]['ansible_facts']['default_ipv4']['address'] }} {{ hostvars[host]['ansible_facts']['fqdn'] }} {{ hostvars[host]['ansible_facts']['hostname'] }} 
{% endfor %} 
````

# Galaxy

#You can list the currently installed system roles with the ansible-galaxy list command 
````
ansible-galaxy list 
- linux-system-roles.kdump, (unknown version) 
- linux-system-roles.network, (unknown version) 
- linux-system-roles.postfix, (unknown version) 
- linux-system-roles.selinux, (unknown version) 
- linux-system-roles.timesync, (unknown version) 
- rhel-system-roles.kdump, (unknown version) 
- rhel-system-roles.network, (unknown version) 
- rhel-system-roles.postfix, (unknown version) 
- rhel-system-roles.selinux, (unknown version) 
- rhel-system-roles.timesync, (unknown version)
````
 
Roles are located in the /usr/share/ansible/roles directory. A role beginning with linux-system-roles is a symlink to the matching rhel-system-roles role. 
 
Review the Role Variables section of the README.md file for the rhel-system-roles.network role. 
cat /usr/share/doc/rhel-system-roles/network/README.md 


# HANDLING TASK FAILURE
Ansible evaluates the return code of each task to determine whether the task succeeded or failed. Normally, when a task fails Ansible immediately aborts the rest of the play on that host, skipping all subsequent tasks. 

## ignore_errors

By default, if a task fails, the play is aborted. However, this behavior can be overridden by ignoring failed tasks. You can use the ignore_errors keyword 

````
- name: Latest version of notapkg is installed 
yum: 
name: notapkg 
state: latest 
ignore_errors: yes
````
## force_handlers
Normally when a task fails and the play aborts on that host, any handlers that had been notified by earlier tasks in the play will not run. If you set the force_handlers: yes keyword on the play, then notified handlers are called even if the play aborted because a later task failed. 
Remember that handlers are notified when a task reports a changed result but are not notified when it reports an ok or failed result.

````
--- 
- hosts: all 
force_handlers: yes 
tasks: 
- name: a task which always notifies its handler 
command: /bin/true 
notify: restart the database 
- name: a task which fails because the package doesn't exist 
yum: 
name: notapkg 
state: latest 
handlers: 
- name: restart the database 
service: 
name: mariadb 

state: restarted ![image](https://github.com/user-attachments/assets/00a7e12c-f7f9-456c-86d8-c3f0f016e891)

````

## failed_when
used with command modules that may successfully execute a command, but the command's output indicates a failure.

````
tasks: 
- name: Run user creation script 
shell: /usr/local/bin/create_users.sh 
register: command_result 
failed_when: "'Password missing' in command_result.stdout"
````

## fail

The fail module can also be used to force a task failure. The above scenario can alternatively be written as two tasks: 
````
tasks: 
- name: Run user creation script 
shell: /usr/local/bin/create_users.sh 
register: command_result 
ignore_errors: yes 
- name: Report script failure 
fail: 
msg: "The password is missing in the output" 
when: "'Password
 missing' in command_result.stdout"
````

# Blocks

locks are clauses that logically group tasks, and can be used to control how tasks are executed. For example, a task block can have a when keyword to apply a conditional to multiple tasks: 
````
- name: block example 
hosts: all 
tasks: 
- name: installing and configuring Yum versionlock plugin 
block: 
- name: package needed by yum 
yum: 
name: yum-plugin-versionlock 
state: present 
- name: lock version of tzdata 
lineinfile: 
dest: /etc/yum/pluginconf.d/versionlock.list 
line: tzdata-2016j-1 
state: present 
when: ansible_distribution == "RedHat" 
````

If any task in a block fails, tasks in its rescue block are executed in order to recover. After the tasks in the block clause run, as well as the tasks in the rescue clause if there was a failure, then tasks in the always clause run.

````
tasks: 
- name: Upgrade DB 
  block: 
  - name: upgrade the database 
    shell: 
      cmd: /usr/local/lib/upgrade-database
  rescue: 
  - name: revert the database upgrade 
    shell: 
      cmd: /usr/local/lib/revert-database 
  always: 
  - name: always restart the database 
    service: 
      name: mariadb 
      state: restarted 
````
 
The when condition on a block clause also applies to its rescue and always clauses if present. 

````
--- 
- name: Task Failure Exercise 
hosts: databases 
vars: 
web_package: http 
db_package: mariadb-server 
db_service: mariadb 
tasks: 
- name: Check local time 
  command: date 
  register: command_result 
  changed_when: false 
- name: Print local time 
  debug: 
    var: command_result.stdout 
- name: Attempt to set up a webserver 
  block: 
  - name: Install {{ web_package }} package 
    yum: 
      name: "{{ web_package }}" 
      state: present 
    failed_when: web_package == "httpd" 
  rescue: 
  - name: Install {{ db_package }} package 
    yum: 
      name: "{{ db_package }}" 
      state: present 
  always: 
  - name: Start {{ db_service }} service 
    service: 
      name: "{{ db_service }}" 
      state: started
````
````
#Block of config tasks 
- name: Setting up the SSL cert directory and config files 
  block: 
  - name: Create SSL cert directory 
    file: 
      path: "{{ ssl_cert_dir }}" 
      state: directory 
  - name: Copy Config Files 
    copy: 
      src: "{{ item.src }}" 
      dest: "{{ item.dest }}" 
  loop: "{{ web_config_files }}" 
  notify: restart web service 
  rescue: 
  - name: Configuration Error Message 
    debug: 
      msg: > 
        One or more of the configuration 
        changes failed, but the web service 
        is still active.
````

# Loops

Ansible supports iterating a task over a set of items using the loop keyword. 
A simple loop iterates a task over a list of items.

The loop variable item holds the value used during each iteration. 

 ````
- name: Postfix and Dovecot are running 
service: 
name: "{{ item }}" 
state: started 
loop: 
- postfix 
- dovecot


- name: Users exist and are in the correct groups 
  user: 
    name: "{{ item.name }}" 
    state: present 
    groups: "{{ item.groups }}" 
  loop: 
  - name: jane 
    groups: wheel 
  - name: joe 
    groups: root
````

# Conditionals

Use conditionals to execute tasks or plays when certain conditions are met.
The when statement is used to run a task conditionally. It takes as a value the condition to test. If the condition is met, the task runs. If the condition is not met, the task is skipped. 

* Equal (value is a string) : ansible_machine == "x86_64" 
* Equal (value is numeric) : max_memory == 512 
* Less than: min_memory < 128 
* Greater than:  min_memory > 256 
* Less than or equal to:  min_memory <= 256 
* Greater than or equal to:  min_memory >= 512
* Not equal to min_memory: != 512 
* Variable exists min_memory: is defined 
* Variable does not exist  min_memory: is not defined 
* Boolean variable is true. The values of 1, True, or yes evaluate to true. : memory_available 
* Boolean variable is false. The values of 0, False, or no evaluate to false.: not memory_available 
* First variable's value is present as a value in second variable's list : ansible_distribution in supported_distros 

when: > 
ansible_memtotal_mb < min_ram_mb or 
ansible_distribution != "RedHat" 
 ![image](https://github.com/user-attachments/assets/136c1ca8-3cd4-40fd-8e7c-282e41461441)


# Vaults

Ansible Vault, which is included with Ansible, can be used to encrypt and decrypt any structured data file used by Ansible. 

````
ansible-vault create filename
ansible-vault create --vault-password-file=vault-pass secret.yml
ansible-vault view secret1.yml
ansible-vault encrypt filename
ansible-vault decrypt secret1.yml --output=secret1-decrypted.yml
ansible-playbook --vault-password-file=vault-pass create_users.yml 
ansible-vault rekey  --new-vault-password-file=NEW_VAULT_PASSWORD_FILE secret.yml 
ansible-playbook --vault-id @prompt site.yml
ansible-playbook \ 
> --vault-id one@prompt --vault-id two@prompt site.yml 
Vault password (one): 
Vault password (two): 
echo -n "NoLoConoceNadie" | ansible-vault encrypt_string --stdin-name "mi_password_secreto"
````
In this scenario, the advantage is that most variables for demo.example.com can be placed in the vars file, but sensitive variables can be kept secret by placing them separately in the vault file. Then the administrator can use ansible-vault to encrypt the vault file, while leaving the vars file as plain text

````
├── host_vars 
│   └── demo.example.com 
│       ├── vars 
│       └── vault 
├── inventory 
└── playbook.yml 
````

# SSH

To make sure that SSH only tries to authenticate by password and not by an SSH key, use the -o PreferredAuthentications=password option when you log in. 
Log off from servera when you have successfully logged in. 
````
ssh -o PreferredAuthentications=password ansibleuser1@servera.lab.example.com
````
````
ssh-copy-id root@web1.example.com 
echo "automation ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/automation 
echo devops | passwd --stdin automation 
subscription-manager register --username=user.developer --password=12345678 
````
# Task Execution
It is a good practice to organize your play in the order of execution: pre_tasks, roles, tasks, and post_tasks. 
This order comes from the fact that each play listed in the playbook is a YAML dictionary of key-value pairs. The order of the top-level directives (name, hosts, tasks, roles, and so on) is arbitrary, but Ansible handles them in a standardized order when it parses and runs the play.
## Pre_tasks

````
  pre_tasks:
    - name: "Comprobando ansible_limit"
      fail:
        msg: "Se deben indicar en el limit las máquinas o grupos de máquinas sobre los que se quiere ejecutar el playbook"
      when: ansible_limit is not defined
      run_once: True
````

# Handlers

````
  ---
- name: Implementing Handlers
  hosts: web_servers

  pre_tasks:
    - name: Configuring Apache
      ansible.builtin.include_tasks: apache.yml

    - name: Setting web port and zone for firewall
      ansible.builtin.set_fact:
        web_port: 80/tcp
        web_zone: public
      changed_when: true
      notify: display variables

  roles:
    - role: firewall

  post_tasks:
    - name: Ensure the web content is copied
      ansible.builtin.copy:
        src: index.html
        dest: /var/www/html/
      notify: verify connectivity

  handlers:
    - name: Showing the web port configured as pre_task
      ansible.builtin.debug:
        var: web_port
      listen: display variables

    - name: Showing the web zone configured as pre_task
      ansible.builtin.debug:
        var: web_zone
      listen: display variables

    - name: verify connectivity
      ansible.builtin.uri:
        url: http://{​{ ansible_facts['fqdn'] }​}
        status_code: 200
      become: false
````
