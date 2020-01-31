# ansible_fundamentals
earn how to automate your CI/CD pipeline with Ansible.Continuous delivery (CD) means frequently delivering updates to your software application. The idea is that by updating more often, you do not have to wait for a specific timed period, and your organization gets better at the process of responding to change.

![blueprint your infrastructure](https://github.com/boschpeter/ansible_fundamentals/blob/master/pictures/blueprinting.PNG)


# Adding another layer – the MySQL role

we have been focusing on the single tier of our infrastructure, that is, the web server layer. Writing code for just one tier is not a lot of fun. Being a cool DevOps team, we will create a multi-tier infrastructure with database, web server, and then, a load balancer. We will start creating the MySQL role next, apply everything that we have learnt so far, and extend that knowledge with a few new concepts.Here is our specification for the MySQL role:It should install the MySQL server packageIt should configure 'my.cnf', which is the main configuration for the MySQL serverIt should start the MySQL server daemonIt should support Ubuntu 12.04 as well as CentOS/RedHat Enterprise 6.xCreating the scaffolding for the roles with Ansible-GalaxySo far, we have been doing all the hard work to understand and create the directory structure required by the roles. However, to make our lives easier, Ansible ships with a tool called Ansible-Galaxy, which should help us initialize a role by creating the scaffolding automatically and could help us follow the best practices. Ansible-Galaxy actually does more than that. It's a utility to connect to the repository of the freely available Ansible roles hosted at http://galaxy.ansible.com. This is similar to the way we use CPAN or RubyGems.Let's start by initializing the MySQL 

````ansible-galaxy init --init-path  roles/ mysql````

#  Creating variables inside the  mysql/defaults/main.yml file:
We will begin with the creation of variables. Let's set up the sane defaults for Debian/Ubuntu 
inside the 
/mysql/defaults/main.yml file:

````
#/home/boscp08/Projects/scratch/virtual-insanity/ansible_fundamentals/Ansible_playbook_essentials_03_code/roles/mysql/defaults
---
mysql_user: mysql
mysql_port: 3306
mysql_datadir: /var/lib/mysql
mysql_bind: 127.0.0.1
mysql_pkg: mysql-server
mysql_pid: /var/run/mysqld/mysqld.pid
mysql_socket: /var/run/mysqld/mysqld.sock
mysql_cnfpath: /etc/mysql/my.cnf
mysql_service: mysql
````

#r User-defined variables
We looked at facts, which are automatically available, and the amount of data that is discovered is overwhelming. However, it does not provide us with every attribute of our infrastructure that we need. For example, Ansible can not discover:Which port we want our web server to listen to

Where a variable can be defined from is a complex phenomenon, as Ansible offers abundant choices in this regard. This also offers a lot of flexibility to users to configure portions of their infrastructures divergently. For example, all Linux hosts in a production environment should use local package repositories or web servers in staging and should run on the port 8080. All this without changing the code, and driven by data alone is done, by variables.The following are the places from where Ansible accepts variables:The default directory inside a roleInventory variablesThe host_vars and group_vars parameters defined in separate directoriesThe host/group vars parameter defined in an inventory fileVariables in playbooks and role parametersThe vars directory inside a role and variables defined inside a playExtra variables provided with the -e option at runtime



# Install, configure, and start the MySQL service on the database servers

# Install and configure the web servers that run Nginx .
 
# Deploy a Socialcoin-api on the web servers and add respective configurations to Nginx.

# Start the Nginx service on all web servers after deploying socialcoin-api. 

The following is a sample playbook that translates the infrastructure blueprint into policies enforceable by Ansible:

![sample_playbook](https://github.com/boschpeter/ansible_fundamentals/blob/master/pictures/sample_playbook.PNG)

## playbook
A playbook consists of one or more plays, which map groups of hosts to well-defined tasks. The preceding example contains three plays, each to configure one layer in the multitiered web application. Plays also define the order in which tasks are configured. This allows us to orchestrate multitier deployments. For example, configure the load balancers only after starting the web servers, or perform two-phase deployment where the first phase only adds this configurations and the second phase starts the services in the desired order.YAML – the playbook languageAs you may have already noticed, the playbook that we wrote previously resembles more of a text configuration than a code snippet. This is because the creators of Ansible chose to use a simple, human-readable, and familiar YAML format to blueprint the infrastructure. This adds to Ansible's appeal, as users of this tool need not learn any special programming language to get started with. Ansible code is self-explanatory and self-documenting in nature. A quick crash course on YAML should suffice to understand the basic syntax. Here is what you need to know about YAML to get started with your first playbook:The first line of a playbook should begin with "--- " (three hyphens) which indicates the beginning of the YAML document.Lists in YAML are represented with a hyphen followed by a white space. A playbook contains a list of plays; they are represented with "- ". Each play is an associative array, a dictionary, or a map in terms of key-value pairs.Indentations are important. All members of a list should be at the same indentation level.Each play can contain key-value pairs separated by ":" to denote hosts, variables, roles, tasks, and so on.

## tasks 
Tasks are a sequence of actions performed against a group of hosts that match the pattern specified in a play. Each play typically contains multiple tasks that are run serially on each machine that matches the pattern. For example, take a look at the following code snippet:

# Bringing In Your Code 
– Custom Commands and ScriptsAnsible comes with a wide variety of built-in modules that allow us to manage various system components, for example, users, packages, network, files, and services. Ansible's battery-included approach also provides the ability to integrate the components with cloud platforms, databases, and applications such as Jira, Apache, IRC, and Nagios, and so on. However, every now and then, we would find ourselves in a position where we may not find a module that exactly does the job for us. For example, installing a package from source involves downloading it, extracting a source tarball, followed by the make command, and finally, "make install". 
There is no single module that does this. There will also be times when we would like to bring in our existing scripts that we have spent nights creating and just have them invoked or scheduled with Ansible, for example, nightly backup scripts. Ansible's command modules would come to our rescue in such situations.In this chapter, we are going to introduce you to:How to run custom commands and scriptsAnsible command modules: raw, command, shell, and scriptHow to control the idempotence of a command moduleRegistered variablesHow to create a socialcoin application

# The command modules

This is the most recommended module for executing commands on target nodes. This module takes the free-form command sequence and allows you to run any command that could be launched from a command-line interface. In addition to the command, we could optionally specify:Which directory to run the command fromWhich shell to use for executionWhen not to run 

 ````
 -name: run a command on target node 
  ccommand: ls -ltr 
  args: 
    chdir: /etcHere
  ````
  
 a command module is called to run ls -ltr on the target hosts with an argument to change the directory to /etc before running the command.In addition to writing it as a task, the command module can directly be invoked as: $ 
 
 ```ansible -i customhosts all -m command -a "ls -ltr"````
 
 


## raw

A raw module can be used to connect to the target host and install the prerequisite package before executing any Ansible code.In the case of network devices, such as routers, switches, and other embedded systems, Python may not be present at all. These devices can still be managed with Ansible simply using a raw module.Apart from these exceptions, for all other cases, it is recommended that you use either command or shell modules, as they offer ways to control when, from where, and how the commands are run.Let's take a look at the following given examples: $ 
````ansible -i customhosts all -m raw -a "uptime" ````

the preceding command connects to all the hosts in the inventory provided with customhosts using SSH, runs a raw command uptime, and returns the results. This would work even if the target host does not have Python installed. This is equivalent to writing a for loop to an ad hoc shell command on a group of hosts.The same command can be converted to a task as: 
````
- name: running a raw command 
  raw: uptime
````


## command

This is the most recommended module for executing commands on target nodes. This module takes the free-form command sequence and allows you to run any command that could be launched from a command-line interface. In addition to the command, we could optionally specify:Which directory to run the command from Which shell to use for executionWhen not to run the commands
Let's take a look at the following example:

a command module is called to run ls -ltr on the target hosts with an argument to change the directory to /etc before running the command. In addition to writing it as a task, 
 
 ````the command module can directly be invoked as:  $ ansible -i customhosts all -m command -a "ls -ltr"````

````
 -name: run a command on target node 
  command: ls -ltr 
  args: 
   chdir: /etcHere
  ```` 
 
## shell
and module we just learnt about. It takes a free-form command and optional parameters and executes them on the target node. However, there are subtle differences between shell modules and command modules, which are listed, as follows:Shell runs the command through the '/bin/sh' shell on the target host, which also means that any command that gets executed with this module has access to all the shell variables on that systemUnlike the command module, shell also allows the usage of operators, such as redirects 
Shell is less secure than a command module, as it can be affected by a shell environment on the remote hostLet's take a look at the following example:
````
- name: run a shell command on target node s
  shell: ls -ltr | grep host  /tmp/hostconfigs 
  args: 
   chdir: /etc
````
