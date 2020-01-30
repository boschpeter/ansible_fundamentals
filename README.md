# ansible_fundamentals
earn how to automate your CI/CD pipeline with Ansible.Continuous delivery (CD) means frequently delivering updates to your software application. The idea is that by updating more often, you do not have to wait for a specific timed period, and your organization gets better at the process of responding to change.

![blueprint your infrastructure](https://github.com/boschpeter/ansible_fundamentals/blob/master/pictures/blueprinting.PNG)


# Adding another layer – the MySQL role

we have been focusing on the single tier of our infrastructure, that is, the web server layer. Writing code for just one tier is not a lot of fun. Being a cool DevOps team, we will create a multi-tier infrastructure with database, web server, and then, a load balancer. We will start creating the MySQL role next, apply everything that we have learnt so far, and extend that knowledge with a few new concepts.Here is our specification for the MySQL role:It should install the MySQL server packageIt should configure 'my.cnf', which is the main configuration for the MySQL serverIt should start the MySQL server daemonIt should support Ubuntu 12.04 as well as CentOS/RedHat Enterprise 6.xCreating the scaffolding for the roles with Ansible-GalaxySo far, we have been doing all the hard work to understand and create the directory structure required by the roles. However, to make our lives easier, Ansible ships with a tool called Ansible-Galaxy, which should help us initialize a role by creating the scaffolding automatically and could help us follow the best practices. Ansible-Galaxy actually does more than that. It's a utility to connect to the repository of the freely available Ansible roles hosted at http://galaxy.ansible.com. This is similar to the way we use CPAN or RubyGems.Let's start by initializing the MySQL 

````ansible-galaxy init --init-path  roles/ mysql````


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
