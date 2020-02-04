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

to be data driven. We will start templating the default.conf file for the role . The approach toward converting a file into a template would be as follows:Create the directories required to hold templates and default variables inside a role: 

````
$ mkdir roles/nginx/templates 
$ mkdir roles/nginx/default
````
# templating

Create roles/nginx/defaults/main.yml and store the sane defaults as follows
````
---
#file: roles/nginx/defaults/main.yml 
nginx_port: 80 
nginx_root: /usr/share/nginx/html 
nginx_index: index.html
````

change the task in the configure.yml file to use the template instead of the copy module:
````
# filename: roles/mysql/tasks/configure.yml
 - name: create config
   template: src="my.cnf" dest="{{ mysql_cnfpath }}" mode=0644
````



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


![panda](  https://github.com/boschpeter/ansible/blob/master/pictures/08B0E82E-5268-4BFD-96F6-99527EC96885.jpeg)

# ansible

Met Ansible kunt u een hele omgeving voorzien van een enkele opdracht en enkele eenvoudige SSH-opdrachten automatiseren. Automatische implementaties zijn tegenwoordig in zwang. Het lijkt steeds relevanter te worden om te weten hoe automatisering moet worden aangepakt. Ansible is een Zwitsers zakmes voor DevOps, in staat om veel krachtige automatiseringstaken uit te voeren met de flexibiliteit om zich aan te passen aan vele omgevingen en workflows. Aan de hand van voorbeelden uit de echte wereld deelt dezer repo hopelijk "de essentie" voor alle Ansible-gebruikers. Of je nu gloednieuw bent bij Ansible of op zoek bent naar een opfriscursus, de wiki  biedt de best practices die je moet weten. (dat is tenminste mijn gedachte)
Lessons-learned 

Ansible is een radicaal eenvoudig IT-automatiseringsplatform dat uw applicaties en systemen eenvoudiger inzetbaar maakt. Vermijd het schrijven van scripts of aangepaste code om uw applicaties te implementeren en bij te werken - automatiseer in een taal die gewoon Engels benadert, met behulp van SSH, zonder agenten om op externe systemen te installeren

# git init

````
git init
git config --global credential.helper store
git config --global user.email "bosch.peter@icloud.com"
git config --global user.name "boschpeter"
git config --global user.password "

git config --list
`````
 De echte magie van infrastructuur als codetools, zoals Ansible, ligt in het vermogen om gegevens en code te scheiden. In het voorbeeld is het bestand default.conf een configuratiebestand dat specifiek is voor een Nginx-webserver. De configuratieparameters, zoals poorten, gebruikers, paden, enzovoort, blijven te allen tijde generiek en constant, ongeacht wie ze installeert en configureert. Wat niet constant is, zijn de waarden die deze parameters aannemen. Dat is wat specifiek is voor onze organisatie. Dus, hiervoor zouden we het volgende beslissen: Welke poort moet Nginx gebruiken? Welke gebruiker moet eigenaar zijn van het webserverproces? Waar moeten de logboekbestanden naartoe? Hoeveel werkprocessen moeten worden uitgevoerd? Ons organisatiespecifieke beleid kan ook vereisen ons om verschillende waarden door te geven aan deze parameters op basis van de omgeving of geografie waarin de hosts draaien. Kan splitsen deze in twee delen: De code die generiek is De gegevens die specifiek zijn voor een organisatie Dit heeft twee voordelen; een voordeel is dat het ons probleem van statische gegevensexplosie oplost. Nu we de code en gegevens hebben gescheiden, kunnen we configuratiebestanden flexibel en dynamisch maken. Het tweede voordeel, u zich misschien realiseren, is nu dat de code en gegevens zijn gesplitst, er is niets in de code dat specifiek is voor een bepaalde organisatie. Dit maakt het gemakkelijk om de site met de wereld te delen voor iedereen die het nuttig vindt. Dat is precies wat je zou vinden op Ansible-Galaxy of zelfs op GitHub, wat de groei van tools, zoals Ansible, stimuleert. In plaats van het wiel opnieuw uit te vinden, kunt u de code downloaden die iemand anders heeft geschreven, deze aanpassen, de gegevens voor de code invullen en het werk doen
 
Hoe staat deze code los van de gegevens? Het antwoord is dat Ansible twee primitieven heeft: Jinja-sjablonen (code) De variabelen (gegevens) In het volgende diagram wordt uitgelegd hoe het resulterende bestand wordt gegenereerd op basis van sjablonen en variabelen: Sjablonen bieden plaatshouders in plaats van parameterwaarden, die vervolgens worden gedefinieerd in variabelen. Variabelen kunnen vervolgens vanuit verschillende plaatsen worden ingevoerd, waaronder rollen, playbooks, inventarissen en zelfs vanaf de opdrachtregel wanneer u Ansible start.

![j2](https://github.com/boschpeter/ansible/blob/master/pictures/file_generated_from_template_and_variables.PNG)

# Ninja2 templates 
Waar gaat Jinja over? Jinja2 is een zeer populaire en krachtige op Python gebaseerde sjabloon-engine. Aangezien Ansible in Python is geschreven, wordt het de standaardkeuze voor de meeste gebruikers, net als andere op Python gebaseerde configuratiebeheersystemen, zoals Fabric en SaltStack. De naam Jinja is afkomstig van het Japanse woord voor tempel, wat qua fonetiek vergelijkbaar is met de woordsjabloon. Enkele belangrijke kenmerken van Jinja2 zijn: het is snel en precies op tijd gecompileerd met de Python-bytecode Het heeft een optionele sandbox-omgeving Het is gemakkelijk naar debugIt ondersteunt template-overervingThe template-formatie Templates lijken erg op normale tekstgebaseerde bestanden behalve de occasionele variabelen of code die de speciale tags omgeeft. Deze worden geëvalueerd en worden meestal vervangen door waarden tijdens runtime, waardoor een tekstbestand wordt gemaakt dat vervolgens naar de doelhost wordt gekopieerd. Dit zijn de twee soorten tags die Jinja2-sjablonen accepteren: {{}} sluit variabelen in een sjabloon in en drukt de waarde ervan af in het resulterende bestand. Dit is het meest voorkomende gebruik van een sjabloon. Bijvoorbeeld: {{nginx_port}}
 {%%} sluit code-instructies in een sjabloon in, bijvoorbeeld voor een lus worden de if-else-instructies ingesloten, die tijdens runtime worden geëvalueerd maar niet worden afgedrukt
 
 # Facts
 Veel gegevens in onze systemen worden automatisch ontdekt en beschikbaar gesteld aan Ansible door de beheerde hosts tijdens het handshake-proces. Deze gegevens zijn zeer nuttig en vertellen ons alles over dat systeem, zoals: De hostnaam, netwerkinterface en IP-adres De systeemarchitectuur Het besturingssysteem De schijven De gebruikte processor en de hoeveelheid geheugen Of het nu een VM is; zo ja, is het een virtualisatie- / cloudprovider? TipFacts worden verzameld aan het begin van een Ansible-run. Herinner je je de regel in de uitvoer die VERZAMELENDE FEITEN ******* zegt? Dat is precies wanneer dit gebeurt.
 
# Variabelen 

![var_precedence](https://github.com/boschpeter/ansible/blob/master/pictures/code_tree_nginx_role.PNG)


 ## Variablen de default-directory  binnen een rol 
Waar en hoe een variabele kan worden gedefinieerd, is een complex fenomeen, omdat Ansible in dit opzicht overvloedige keuzes biedt. Dit biedt gebruikers ook veel flexibiliteit om delen van hun infrastructuur verschillend te configureren. Alle Linux-hosts in een productieomgeving moeten bijvoorbeeld lokale pakketrepository's of webservers gebruiken voor staging en moeten op poort 8080 worden uitgevoerd. Dit alles zonder de code te wijzigen, en alleen aangestuurd door gegevens, gebeurt door variabelen. plaatsen waar Ansible variabelen accepteert: de default-directory  binnen een rol 

## Inventory variabelen 
De parameters host_vars en group_vars gedefinieerd in afzonderlijke mappen. De parameter host / group vars gedefinieerd in een inventarisbestand Variabelen in playbooks en rolparameters De map vars in een rol en variabelen gedefinieerd in een verstrekte play 

##  Extra-variabelen met de optie -e tijdens runtime

tot zover  feiten, variabelen en sjablonen.

# Templating the Nginx configurations
Laten we nu onze Nginx-rol transformeren tot gegevensgestuurd. We beginnen met het sjablonen van het default.conf-bestand voor Nginx dat we eerder hebben gemaakt. De aanpak voor het converteren van een bestand naar een sjabloon zou als volgt zijn: M

## 1 maak de mappen die nodig zijn om sjablonen en standaardvariabelen in een rol te houden: 

|create directories|
|--------------------------------|
|mkdir rollen / nginx / templates|
|mkdir rollen / nginx / defaults| 

````ansible-galaxy init --init-path roles/ nginx````


## 2 Begin altijd met het uiteindelijke configuratiebestand, 
Begin altijd met het eigenlijke configuratiebestand, ons eindresultaat van dit proces, om alle parameters te kennen die het zou kosten. relax. De configuratie voor het bestand default.conf op ons systeem is bijvoorbeeld als volgt: 

````
server {
       listen 80; 
       servername localhost; 
     location / {
     root / usr / share / nginx / html; 
     index index.html; 
     }
 } 
````

## 3 Identificeer de configuratieparameters die u dynamisch wilt genereren,

verwijder de waarden voor die parameters, noteer ze afzonderlijk en vervang ze door sjabloonvariabelen: 

Template Snippets: 
 listen {{nginx_port}}
 root {{ nginx_root }}; 
 index {{ nginx_index }}; 
 
 Variables: 
 nginx_port: 80 
 nginx_root: /usr/share/nginx/html 
 nginx_index: index.html
 
De waarden voor een van de configuratieparameters worden verondersteld  afkomstig te zijn van feiten, meestal systeemparameters of topologie-informatie, zoals de hostnaam, het IP-adres, enzovoort, zoek dan de relevante feiten met behulp van de volgende opdracht: Bijvoorbeeld: ````$ ansible -i customhosts www -m setup | less ````

## 4 Gebruik het  ontdekte feit in de sjabloon in plaats van een door de gebruiker gedefinieerde variabele. 

Bijvoorbeeld: servernaam {{ansible_hostname}}, Om de hostnaam van het systeem te achterhalen: 
````ansible -i customhosts dingofarm_workers-m setup | grep -i hostname ````

"ansible_hostname": "ubuntuVBX2, 
"ansible_hostname": "ubuntuVBX", 

## 5 sla het resulterende bestand op in de map van het sjabloon template,  idealiter met de extensie .j2. 

**rollen / nginx / templates / default.conf.j2 **

Voor rollen / nginx / templates / default.conf.j2 wordt het resulterende bestand bijvoorbeeld
rollen / nginx / templates / default.conf.j2 
 
 ````
 server {
    listen {{nginx_port}}; 
    servername {{ansible_hostname}}; 
 
 location / {
   root {{nginx_root}}; 
    index {{nginx_index}}; 
   }
  } 
 ````
 
# 6 en sla de normale standaardwaarden op als volgt: ---
create roles / nginx / defaults / main.yml 
 

#file:

**rollen / nginx / defaults / main.yml **
````
nginx_port: 80 
nginx_root: / usr / share / nginx / html 
nginx_index: index.html
````

Zodra de sjabloon is gemaakt, wijzigt u de taak in het configure.yml-bestand om de sjabloon te gebruiken in plaats van de kopieermodule: 

![tree](https://github.com/boschpeter/ansible/blob/master/pictures/code_tree_nginx_role.PNG)

![templating](https://github.com/boschpeter/ansible/blob/master/pictures/role_nginx_task_configuration.PNG)
ten slotte is het tijd om eventueel het statische bestand te verwijderen dat we eerder met de kopieermodule hebben gebruikt

![j2_templating](https://github.com/boschpeter/ansible/blob/master/pictures/A0DEB38D-DAF9-45ED-ACBF-EE2418E57D43.jpeg)

#  aanbevelingen voor de beste praktijken bij het gebruik van variabelen: 

begin met standaardwaarden in een rol. Dit heeft de laagste prioriteit van allemaal. Dit is ook een goede plek om de normale standaardwaarden van uw toepassing te bieden, die later op verschillende plaatsen kunnen worden overschreven. Groepsvariabelen zijn erg handig. Vaak zullen we regiospecifieke of omgevingsspecifieke configuraties uitvoeren. We zouden ook bepaalde rollen toepassen op een bepaalde groep servers, bijvoorbeeld voor alle webservers in Azië passen we de Nginx-rol toe. Er is ook een standaardgroep met de naam "alle", die alle hosts voor alle groepen zal bevatten. Het is een goede gewoonte om de variabelen die voor alle groepen gebruikelijk zijn in "alle" (group_vars / all) te plaatsen, die dan door meer specifieke groepen kunnen worden overschreven. Als er host-specifieke uitzonderingen zijn, gebruik dan hosts_vars, bijvoorbeeld host_vars / specialhost. example.org.Als u variabelen in verschillende bestanden wilt scheiden, maakt u mappen met de naam van de hosts en plaatst u de variabele bestanden erin. Alle bestanden in die mappen worden geëvalueerd: group_vars / asia / webhost_vars / specialhost / nginxhost_vars / specialhost / mysqlAls u uw rollen generiek en deelbaar wilt houden, gebruikt u standaardwaarden in de rollen en geeft u vervolgens organisatiespecifieke variabelen op uit playbooks. Deze kunnen worden gespecificeerd als rolparameters. Als u wilt dat rolvariabelen altijd voorrang hebben op voorraadvariabelen en playbooks, geef ze dan op in de vars-directory binnen een rol. Dit is handig voor het leveren van rolconstanten voor specifieke platforms. Ten slotte, als u een van de voorgaande variabelen wilt overschrijven en wat gegevens wilt opgeven tijdens runtime, biedt u een extra variabele met Ansible-opdrachten met de optie -e.

![variable_precedence](https://github.com/boschpeter/ansible/blob/master/pictures/variabe_precedence.PNG)

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
