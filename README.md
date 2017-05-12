# about this repo
Junos automation with stackstorm. 

# about stackstorm
StackStorm is event-driven automation: 
- it doesnt rely on human input. 
- it reacts to events.  

Stackstorm has network automation capabilities with the stackstorm packs napalm and ansible. So stackstorm can fire napalm or ansible content based on events.    

https://keepingitclassless.net/2016/12/introduction-to-stackstorm/ (Matt Oswalt)  
https://medium.com/netflix-techblog/introducing-winston-event-driven-diagnostic-and-remediation-platform-46ce39aa81cc (netflix)  


# stackstorm documentation: 

stackstorm documentation: https://docs.stackstorm.com/index.html  
stackstorm exchange (integration packs hub): https://exchange.stackstorm.org/  
github: https://github.com/StackStorm  
slack team: https://stackstorm-community.slack.com  

# stackstorm installation: 

## all-in-one quick installation: 
To quickly install ST2 on ubuntu (single-box deployment for demo), run these commands: 
```
sudo apt-get install curl
curl -sSL https://stackstorm.com/packages/install.sh | bash -s -- --user=st2admin --password=Ch@ngeMe
```
The installation takes about 12 minutes.  
Once it completes successfully, you will see the following output:
```
########################################################


███████╗████████╗██████╗      ██████╗ ██╗  ██╗
██╔════╝╚══██╔══╝╚════██╗    ██╔═══██╗██║ ██╔╝
███████╗   ██║    █████╔╝    ██║   ██║█████╔╝
╚════██║   ██║   ██╔═══╝     ██║   ██║██╔═██╗
███████║   ██║   ███████╗    ╚██████╔╝██║  ██╗
╚══════╝   ╚═╝   ╚══════╝     ╚═════╝ ╚═╝  ╚═╝

  st2 is installed and ready to use.

Head to https://YOUR_HOST_IP/ to access the WebUI

Don't forget to dive into our documentation! Here are some resources
for you:

* Documentation  - https://docs.stackstorm.com
* Knowledge Base - https://stackstorm.reamaze.com

Thanks for installing StackStorm! Come visit us in our Slack Channel
and tell us how it's going. We'd love to hear from you!
```

## Verify: 
```
$ st2 --version
st2 2.2.1
$ st2 --help
```
## User: 
```
$ whoami
ksator
$ st2 whoami 
Currently logged in as st2admin
```

# packs
stackstorm exchange (integration packs hub): https://exchange.stackstorm.org/  
to get the list of installed pack, run this command: 
```
$ sudo st2 pack list
+---------+---------+------------------------------------+---------+------------------+
| ref     | name    | description                        | version | author           |
+---------+---------+------------------------------------+---------+------------------+
| chatops | chatops | Chatops integration pack           | 0.2.0   | Kirill Enykeev   |
| core    | core    | st2 content pack containing basic  | 0.2.1   | st2-dev          |
|         |         | actions.                           |         |                  |
| default | default | Pack where all the resources which | 0.1.0   | st2-dev          |
|         |         | are created using the API and      |         |                  |
|         |         | don't have a pack specified get    |         |                  |
|         |         | saved.                             |         |                  |
| linux   | linux   | Generic linux actions              | 0.1.0   | st2-dev          |
| packs   | packs   | core st2 content pack containing   | 0.2.0   | st2-dev          |
|         |         | pack management functionality.     |         |                  |
| st2     | st2     | StackStorm pack management         | 0.2.0   | StackStorm, Inc. |
+---------+---------+------------------------------------+---------+------------------+
$ 
```
Packs are placed under /opt/stackstorm/packs directory: 
```
$ ls /opt/stackstorm/packs/
chatops  core  default  linux  packs  st2
```

## napalm  
StackStorm pack for working with network devices using the NAPALM python library: https://github.com/StackStorm-Exchange/stackstorm-napalm  
All Python dependencies are included in requirements.txt. These will be installed for you when you install the pack using st2 pack install.    
```
$ sudo st2 pack -h
```
to get the list of installed pack, run this command: 
```
$ sudo st2 pack list
+---------+---------+------------------+---------+------------------+
| ref     | name    | description      | version | author           |
+---------+---------+------------------+---------+------------------+
| chatops | chatops | Chatops          | 0.2.0   | Kirill Enykeev   |
|         |         | integration pack |         |                  |
| core    | core    | st2 content pack | 0.2.1   | st2-dev          |
|         |         | containing basic |         |                  |
|         |         | actions.         |         |                  |
| default | default | Pack where all   | 0.1.0   | st2-dev          |
|         |         | the resources    |         |                  |
|         |         | which are        |         |                  |
|         |         | created using    |         |                  |
|         |         | the API and      |         |                  |
|         |         | don't have a     |         |                  |
|         |         | pack specified   |         |                  |
|         |         | get saved.       |         |                  |
| linux   | linux   | Generic linux    | 0.1.0   | st2-dev          |
|         |         | actions          |         |                  |
| packs   | packs   | core st2 content | 0.2.0   | st2-dev          |
|         |         | pack containing  |         |                  |
|         |         | pack management  |         |                  |
|         |         | functionality.   |         |                  |
| st2     | st2     | StackStorm pack  | 0.2.0   | StackStorm, Inc. |
|         |         | management       |         |                  |
+---------+---------+------------------+---------+------------------+
$
```
to get information about an installed pack, run this command: 
```
$ sudo st2 pack get napalm
Pack "napalm" is not found.
$
```
```
$ sudo st2 pack search napalm

+--------+----------------------+---------+----------------------+
| name   | description          | version | author               |
+--------+----------------------+---------+----------------------+
| napalm | A StackStorm pack    | 0.2.3   | mierdin, Rob         |
|        | for working with     |         | Woodward             |
|        | network devices      |         |                      |
|        | using the NAPALM     |         |                      |
|        | library              |         |                      |
+--------+----------------------+---------+----------------------+
$
```
```
$ sudo st2 pack show napalm
+-------------+--------------------------------------------------------------+
| Property    | Value                                                        |
+-------------+--------------------------------------------------------------+
| name        | napalm                                                       |
| description | A StackStorm pack for working with network devices using the |
|             | NAPALM library                                               |
| author      | mierdin, Rob Woodward                                        |
| content     | {                                                            |
|             |     "rules": {                                               |
|             |         "count": 3                                           |
|             |     },                                                       |
|             |     "actions": {                                             |
|             |         "count": 27                                          |
|             |     },                                                       |
|             |     "aliases": {                                             |
|             |         "count": 1                                           |
|             |     }                                                        |
|             | }                                                            |
| email       | info@stackstorm.com                                          |
| keywords    | [                                                            |
|             |     "networking",                                            |
|             |     "napalm",                                                |
|             |     "cisco",                                                 |
|             |     "juniper",                                               |
|             |     "arista",                                                |
|             |     "ibm"                                                    |
|             | ]                                                            |
| repo_url    | https://github.com/StackStorm-Exchange/stackstorm-napalm     |
| version     | 0.2.3                                                        |
+-------------+--------------------------------------------------------------+
$ 
```
To install this pack, run this command:
```
sudo st2 pack install napalm
```
```
sudo st2 pack get napalm
```

Packs are placed under /opt/stackstorm/packs directory: 
```
ls /opt/stackstorm/packs/
chatops  core  default  linux  napalm  packs  st2
$
```
```
$ ls -la /opt/stackstorm/packs/napalm/
total 108
drwxrwxr-x 8 root st2packs  4096 mei 12 13:51 .
drwxrwxr-x 9 root st2packs  4096 mei 12 13:51 ..
drwxrwxr-x 5 root st2packs  4096 mei 12 13:51 actions
drwxrwxr-x 2 root st2packs  4096 mei 12 13:51 aliases
-rwxrwxr-x 1 root st2packs   414 mei 12 13:51 circle.yml
-rwxrwxr-x 1 root st2packs   765 mei 12 13:51 config.schema.yaml
drwxrwxr-x 3 root st2packs  4096 mei 12 13:51 examples
drwxrwxr-x 8 root st2packs  4096 mei 12 13:51 .git
-rwxrwxr-x 1 root st2packs  1063 mei 12 13:51 .gitignore
-rwxrwxr-x 1 root st2packs 38031 mei 12 13:51 icon.png
drwxrwxr-x 3 root st2packs  4096 mei 12 13:51 lint-configs
-rwxrwxr-x 1 root st2packs   355 mei 12 13:51 napalm.yaml.example
-rwxrwxr-x 1 root st2packs   267 mei 12 13:51 pack.yaml
-rwxrwxr-x 1 root st2packs  9125 mei 12 13:51 README.md
-rwxrwxr-x 1 root st2packs   155 mei 12 13:51 requirements.txt
drwxrwxr-x 2 root st2packs  4096 mei 12 13:51 rules
$
```

A virtualenv is created for each pack under /opt/stackstorm/virtualenv.  
Python dependencies are installed inside the virtualenv with pip -r requirements.txt.
```
$ more /opt/stackstorm/packs/napalm/requirements.txt 
napalm-base
napalm-eos
napalm-fortios
napalm-ibm
napalm-ios
napalm-iosxr
napalm-junos
napalm-nxos
napalm-pluribus
napalm-panos
napalm
json2table
GitPython
$
```
```
$ pip list | grep "junos\|jxmlease\|napalm\|json2table\|GitPython"
$
```
```
$ /opt/stackstorm/virtualenvs/napalm/bin/pip list | grep "junos\|jxmlease\|napalm\|json2table\|GitPython"
GitPython (2.1.3)
json2table (1.1.5)
junos-eznc (2.1.2)
jxmlease (1.0.1)
napalm (1.2.0)
napalm-base (0.23.3)
napalm-eos (0.5.6)
napalm-fortios (0.4.0)
napalm-ibm (0.1.7)
napalm-ios (0.6.2)
napalm-iosxr (0.4.8)
napalm-junos (0.6.6)
napalm-nxos (0.5.2)
napalm-panos (0.4.0)
napalm-pluribus (0.5.1)
napalm-ros (0.2.2)
napalm-vyos (0.1.3)
$ 
```

to remove this pack, run this command:
```
$ sudo st2 pack remove napalm

```


# Looking for more Junos automation examples:  

Junos automation with Network Infrastructure as Code (git, gitlab, gitlabCI, IaC, CI/CD, pipeline, ansible)  
https://gitlab.com/ksator/network-infrastructure-as-code  

How to automate Openconfig configuration on Juniper devices (openconfig, yang, netconf, pyang, pyangbind, ansible,  pyez)     
https://github.com/ksator/openconfig-demo-with-juniper-devices  

How to automate verifications on Junos devices (jsnapy)  
https://github.com/ksator/junos-verifications-automation-with-jsnapy  

How to delegate junos automation tasks chatting to a bot with slack   
https://github.com/ksator/junos-automation-with-chatops-in-ams  

How to automate junos with ansible  
https://github.com/ksator/ansible-training-for-junos-automation  

How to automate junos with python (pyez, ncclient, napalm)  
https://github.com/ksator/python-training-for-network-engineers  

How to orchestrate Junos virtual machines with Vagrant (vagrant, virtual box, ansible, vsrx, vqfx)  
https://github.com/ksator/vagrant-with-junos  


