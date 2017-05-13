# About this repository
How to automate Junos using StackStorm with the integration packs napalm and ansible.  

# About StackStorm

## What is StackStorm
StackStorm is event-driven automation: it reacts to events, it doesnt require an human input.   

https://keepingitclassless.net/2016/12/introduction-to-stackstorm/ (Matt Oswalt)  
https://medium.com/netflix-techblog/introducing-winston-event-driven-diagnostic-and-remediation-platform-46ce39aa81cc (Netflix)  

## StackStorm documentation 

StackStorm documentation: https://docs.stackstorm.com/index.html  
StackStorm exchange (integration packs hub): https://exchange.stackstorm.org/  
Github: https://github.com/StackStorm  
Slack team: https://stackstorm-community.slack.com  

## Junos automation with StackStorm 
StackStorm has network automation capabilities with the StackStorm integration packs napalm and ansible. So StackStorm can fire NAPALM or Ansible content based on events.  
Junos can be automated with both NAPALM and Ansible.  
You need to enable Netconf on the Junos devices:
```
set system services netconf ssh
commit
```

# StackStorm installation 

## All-in-one quick installation 
Run these commands to execute a deployment script that quickly install StackStorm on ubuntu (single-box deployment for demo).  
It will create the st2 user st2admin with the password Ch@ngeMe. 
```
sudo apt-get install curl
curl -sSL https://stackstorm.com/packages/install.sh | bash -s -- --user=st2admin --password=Ch@ngeMe
```
The installation takes about 10 minutes.  
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

## Verify the StackStorm installation 
```
$ st2 --version
st2 2.2.1
$ st2 --help
```
## User 
```
$ st2 whoami 
Currently logged in as st2admin
$ whoami
ksator
$ echo $USER
ksator
```

# StackStorm integration packs

Integration packs extend StackStorm to integrate it with external systems (Ansible, NAPALM, Puppet ....)  

## Integration packs hub
StackStorm exchange: https://exchange.stackstorm.org/  
StackStorm Exchange is a collection of packs submitted and maintained by the StackStorm community.  

## List of installed packs
to list all installed pack, run the below command. So this is the list of packs installed by default: 
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
```
Installed packs are placed under /opt/stackstorm/packs directory: 
```
$ ls /opt/stackstorm/packs/
chatops  core  default  linux  packs  st2
```
## Discovering available Packs
You can visit the integration packs hub https://exchange.stackstorm.org/  
You can use the command `st2 pack search`. This command searches packs through packs properties: name, description (use quotes for multi-word search), keywords, and even author:
```
$ sudo st2 pack search mierdin
+--------+--------------------------------------------+---------+-----------------------+
| name   | description                                | version | author                |
+--------+--------------------------------------------+---------+-----------------------+
| napalm | A StackStorm pack for working with network | 0.2.3   | mierdin, Rob Woodward |
|        | devices using the NAPALM library           |         |                       |
+--------+--------------------------------------------+---------+-----------------------+
```
Once you know the exact pack name, you can use it with the below command to get more details:
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
```

## Pack configuration files
They are located in `/opt/stackstorm/configs/` directory.  
A pack configuration file is a YAML file which contains pack configuration. It is named `<pack name>.yaml`. 
Some packs require a pack configuration file (napalm) and others do not use a pack configuration file (ansible).  
```
$ ls /opt/stackstorm/configs/
$
```

# napalm pack

This is a StackStorm integration pack for NAPALM  

## About NAPALM
NAPALM (Network Automation and Programmability Abstraction Layer with Multivendor support) is a Python library to interact with different network operating systems.  
napalm source code: https://github.com/napalm-automation/napalm  
napalm doc: https://napalm.readthedocs.io/en/latest/index.html  

It is not required to install NAPALM in order to use the napalm pack (the StackStorm integration pack for NAPALM): 
```
$ pip list | grep napalm
$
```

## About the napalm pack
StackStorm pack for working with network devices using the NAPALM python library.  
source code: https://github.com/StackStorm-Exchange/stackstorm-napalm  
All Python dependencies are included in [requirements.txt](https://github.com/StackStorm-Exchange/stackstorm-napalm/blob/master/requirements.txt). These python libraries will be installed (in a virtualenv) when you will install the pack using st2 pack install.    

## napalm pack installation
to get help with the `st2 pack` command, run this command: 
```
$ sudo st2 pack -h
```
this pack is not installed by default.  
to get information about an installed pack, run this command: 
```
$ sudo st2 pack get napalm
Pack "napalm" is not found.
```
To install the napalm pack, run this command:
```
$ sudo st2 pack install napalm

	[ succeeded ] download pack
	[ succeeded ] make a prerun
	[ succeeded ] install pack dependencies
	[ succeeded ] register pack

+-------------+--------------------------------------------------------------+
| Property    | Value                                                        |
+-------------+--------------------------------------------------------------+
| name        | napalm                                                       |
| description | A StackStorm pack for working with network devices using the |
|             | NAPALM library                                               |
| version     | 0.2.3                                                        |
| author      | mierdin, Rob Woodward                                        |
+-------------+--------------------------------------------------------------+
```
Verify the list of installed pack:  
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
| napalm  | napalm  | A StackStorm     | 0.2.3   | mierdin, Rob     |
|         |         | pack for working |         | Woodward         |
|         |         | with network     |         |                  |
|         |         | devices using    |         |                  |
|         |         | the NAPALM       |         |                  |
|         |         | library          |         |                  |
| packs   | packs   | core st2 content | 0.2.0   | st2-dev          |
|         |         | pack containing  |         |                  |
|         |         | pack management  |         |                  |
|         |         | functionality.   |         |                  |
| st2     | st2     | StackStorm pack  | 0.2.0   | StackStorm, Inc. |
|         |         | management       |         |                  |
+---------+---------+------------------+---------+------------------+
```
Get detailed information about an installed pack:
```
$ sudo st2 pack get napalm
+-------------+--------------------------------------------------------------+
| Property    | Value                                                        |
+-------------+--------------------------------------------------------------+
| name        | napalm                                                       |
| version     | 0.2.3                                                        |
| author      | mierdin, Rob Woodward                                        |
| email       | info@stackstorm.com                                          |
| keywords    | [                                                            |
|             |     "networking",                                            |
|             |     "napalm",                                                |
|             |     "cisco",                                                 |
|             |     "juniper",                                               |
|             |     "arista",                                                |
|             |     "ibm"                                                    |
|             | ]                                                            |
| description | A StackStorm pack for working with network devices using the |
|             | NAPALM library                                               |
+-------------+--------------------------------------------------------------+ 
```
Installed packs are placed under the directory `/opt/stackstorm/packs`: 
```
ls /opt/stackstorm/packs/
chatops  core  default  linux  napalm  packs  st2
```
the repo https://github.com/StackStorm-Exchange/stackstorm-napalm  is cloned under the directory `/opt/stackstorm/packs/napalm/` 
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
```

A virtualenv is created for each pack under `/opt/stackstorm/virtualenv`.  
Python dependencies are installed inside the virtualenv with `pip -r requirements.txt`.
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
```
```
$ pip list | grep "junos\|jxmlease\|napalm\|json2table\|GitPython"
$
```
virtualenv for this pack: 
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
```

to remove this pack, run this command:
```
$ sudo st2 pack remove napalm
```
## napalm pack configuration file  
A pack configuration file (`/opt/stackstorm/configs/napalm.yaml`) is required for this pack.  
This is where you tell to StackStorm about the network devices to use and the credentails for logging into the devices.  
You need to create this `napalm.yaml` file.  
The file [napalm.yaml.example](https://github.com/StackStorm-Exchange/stackstorm-napalm/blob/master/napalm.yaml.example) at the root of the pack repository is an example.  
Here's the one I am using:   
```
$ more /opt/stackstorm/configs/napalm.yaml
---
html_table_class: napalm
config_repo: https://github.com/ksator/ex4300-configs.git

credentials:
  core:
    username: pytraining
    password: Poclab123

devices:
- hostname: ex4300-9
  driver: junos
  credentials: core
- hostname: ex4300-17
  driver: junos
  credentials: core
- hostname: ex4300-18
  driver: junos
  credentials: core
```
If the hostnames above do not resolve to IP, update the ```/etc/hosts``` file.
```
$ more /etc/hosts | grep ex4300
172.30.179.95	ex4300-9
172.30.179.74	ex4300-18    
172.30.179.73	ex4300-17    
```

Then you need to register the configuration file (each time you change a configuration file) using this command:
```
sudo st2ctl reload --register-configs
```
you can also use of this command:
```
sudo st2 pack register napalm
```
## list of available actions with the integration pack napalm

To get the list of available actions with the integration pack napalm, run this command: 
```
$ sudo st2 action list -p napalm
+--------------------------------------+--------+----------------------------------------------------------+
| ref                                  | pack   | description                                              |
+--------------------------------------+--------+----------------------------------------------------------+
| napalm.bgp_prefix_exceeded_chain     | napalm | Action Chain to process a BGP neighbour prefix limit     |
|                                      |        | exceeded event.                                          |
| napalm.check_consistency             | napalm | Check that the device's configuration is consistent with |
|                                      |        | the 'golden' config in a Git repository                  |
| napalm.cli                           | napalm | Run CLI commands on a device using NAPALM.               |
| napalm.configuration_change_workflow | napalm | Workflow to process a configuration change on a device.  |
| napalm.get_arp_table                 | napalm | Get the ARP table from a device using NAPALM.            |
| napalm.get_bgp_config                | napalm | Get BGP configuration from a device using NAPALM.        |
| napalm.get_bgp_neighbors             | napalm | Get the BGP neighbors from a device using NAPALM.        |
| napalm.get_bgp_neighbors_detail      | napalm | Get a detailed BGP neighbor from a device using NAPALM.  |
| napalm.get_config                    | napalm | Get configuration from the device using NAPALM.          |
| napalm.get_environment               | napalm | Get the environment sensor output from a device using    |
|                                      |        | NAPALM.                                                  |
| napalm.get_facts                     | napalm | Get the various facts (Version, Serial Number, Vendor,   |
|                                      |        | Model, etc.) from a device using NAPALM.                 |
| napalm.get_firewall_policies         | napalm | Get firewall policies from a device using NAPALM.        |
| napalm.get_interfaces                | napalm | Get interfaces from a device using NAPALM.               |
| napalm.get_lldp_neighbors            | napalm | Get the LLDP Neighbours from a device using NAPALM.      |
| napalm.get_log                       | napalm | Get logs from devices using NAPALM.                      |
| napalm.get_mac_address_table         | napalm | Get the MAC Address table from a device using NAPALM.    |
| napalm.get_network_instances         | napalm | Get details of network/routing instances/vrfs from a     |
|                                      |        | network device using NAPALM.                             |
| napalm.get_ntp                       | napalm | Gets NTP information from a network device using NAPALM. |
| napalm.get_optics                    | napalm | Fetches the power usage on the various transceivers      |
|                                      |        | installed on the device (in dbm) using NAPALM.           |
| napalm.get_probes_config             | napalm | Get RPM (JunOS) or IP SLA (IOS-XR) probe configuration   |
|                                      |        | from a device using NAPALM.                              |
| napalm.get_probes_results            | napalm | Get RPM (JunOS) or IP SLA (IOS-XR) probe results from a  |
|                                      |        | device using NAPALM.                                     |
| napalm.get_route_to                  | napalm | Shows an IP route on a network device using NAPALM.      |
| napalm.get_snmp_information          | napalm | Get the SNMP information from a device using NAPALM.     |
| napalm.interface_down_workflow       | napalm | Workflow to process an interface down event on a device. |
| napalm.loadconfig                    | napalm | Loads (merge) a configuration to a network device using  |
|                                      |        | NAPALM.                                                  |
| napalm.ping                          | napalm | Run a ping from a network device using NAPALM.           |
| napalm.traceroute                    | napalm | Run a traceroute from a network device using NAPALM.     |
+--------------------------------------+--------+----------------------------------------------------------+
```
To get the details about one action, run one of the two below commands (I prefer the second one):  
```
$ sudo st2 action get napalm.get_facts
+-------------+--------------------------------------------------------------+
| Property    | Value                                                        |
+-------------+--------------------------------------------------------------+
| id          | 5907a0d5a374d84b3cc1f4a0                                     |
| uid         | action:napalm:get_facts                                      |
| ref         | napalm.get_facts                                             |
| pack        | napalm                                                       |
| name        | get_facts                                                    |
| description | Get the various facts (Version, Serial Number, Vendor,       |
|             | Model, etc.) from a device using NAPALM.                     |
| enabled     | True                                                         |
| entry_point | get_facts.py                                                 |
| runner_type | python-script                                                |
| parameters  | {                                                            |
|             |     "credentials": {                                         |
|             |         "position": 3,                                       |
|             |         "required": false,                                   |
|             |         "type": "string",                                    |
|             |         "description": "The credentials group which contains |
|             | the username and password to log in"                         |
|             |     },                                                       |
|             |     "htmlout": {                                             |
|             |         "position": 4,                                       |
|             |         "required": false,                                   |
|             |         "type": "boolean",                                   |
|             |         "description": "In addition to the normal output     |
|             | also includes html output."                                  |
|             |     },                                                       |
|             |     "hostname": {                                            |
|             |         "position": 0,                                       |
|             |         "required": true,                                    |
|             |         "type": "string",                                    |
|             |         "description": "The hostname of the device to        |
|             | connect to. Driver must be specified if hostname is not in   |
|             | configuration. Hostname without FQDN can be given if device  |
|             | is in configuration."                                        |
|             |     },                                                       |
|             |     "driver": {                                              |
|             |         "position": 1,                                       |
|             |         "required": false,                                   |
|             |         "type": "string",                                    |
|             |         "description": "Device driver name for connecting to |
|             | device, see                                                  |
|             | https://napalm.readthedocs.io/en/latest/support/index.html   |
|             | for list."                                                   |
|             |     },                                                       |
|             |     "port": {                                                |
|             |         "position": 2,                                       |
|             |         "required": false,                                   |
|             |         "type": "string",                                    |
|             |         "description": "port for accessing device"           |
|             |     }                                                        |
|             | }                                                            |
| notify      |                                                              |
| tags        |                                                              |
+-------------+--------------------------------------------------------------+ 
```
```
$ sudo st2 run napalm.get_facts -h

Get the various facts (Version, Serial Number, Vendor, Model, etc.)
from a device using NAPALM.

Required Parameters:
    hostname
        The hostname of the device to connect to. Driver must be specified if
        hostname is not in configuration. Hostname without FQDN can be given
        if device is in configuration.
        Type: string

Optional Parameters:
    driver
        Device driver name for connecting to device, see
        https://napalm.readthedocs.io/en/latest/support/index.html for list.
        Type: string

    port
        port for accessing device
        Type: string

    credentials
        The credentials group which contains the username and password to log
        in
        Type: string

    htmlout
        In addition to the normal output also includes html output.
        Type: boolean

    env
        Environment variables which will be available to the script.
        Type: object

    timeout
        Action timeout in seconds. Action will get killed if it doesn't finish
        in timeout seconds.
        Type: integer
        Default: 600
```
## Execute actions from napalm pack using StackStorm CLI

napalm.get_facts
```
$ sudo st2 run napalm.get_facts hostname=ex4300-17
....
id: 5915fa85a374d80940a8c67f
status: succeeded
parameters: 
  hostname: ex4300-17
result: 
  exit_code: 0
  result:
    raw:
      fqdn: ex4300-17.poc-nl.jnpr.net
      hostname: ex4300-17
      interface_list:
      - ge-0/0/0
      - pfe-0/0/0
      - pfh-0/0/0
      - ge-0/0/1
      - ge-0/0/2
      - ge-0/0/3
      - ge-0/0/4
      - ge-0/0/5
      - ge-0/0/6
      - ge-0/0/7
      - ge-0/0/8
      - ge-0/0/9
      - ge-0/0/10
      - ge-0/0/11
      - ge-0/0/12
      - ge-0/0/13
      - ge-0/0/14
      - ge-0/0/15
      - ge-0/0/16
      - ge-0/0/17
      - ge-0/0/18
      - ge-0/0/19
      - ge-0/0/20
      - ge-0/0/21
      - ge-0/0/22
      - ge-0/0/23
      - .local.
      - bme0
      - dsc
      - gre
      - ipip
      - irb
      - jsrv
      - lo0
      - lsi
      - me0
      - mtun
      - pimd
      - pime
      - tap
      - vme
      model: EX4300-24T
      os_version: 15.1R5.5
      serial_number: PG3713430170
      uptime: 11147520
      vendor: Juniper
  stderr: ''
  stdout: ''
```
napalm.get_bgp_neighbors_detail
```
$ sudo st2 run napalm.get_bgp_neighbors_detail hostname=ex4300-9 neighbor=192.168.0.4
..
id: 5916200aa374d80940a8c68b
status: succeeded
parameters: 
  hostname: ex4300-9
  neighbor: 192.168.0.4
result: 
  exit_code: 0
  result:
    raw:
      inet.0:
        '110':
        - accepted_prefix_count: 5
          active_prefix_count: 2
          advertised_prefix_count: 5
          configured_holdtime: 90
          configured_keepalive: 30
          connection_state: Established
          export_policy: bgp-out
          flap_count: 0
          holdtime: 90
          import_policy: bgp-in
          input_messages: 236802
          input_updates: 3
          keepalive: 30
          last_event: RecvKeepAlive
          local_address: 192.168.0.5
          local_address_configured: false
          local_as: 109
          local_as_prepend: true
          local_port: 61847
          messages_queued_out: 0
          multihop: false
          multipath: true
          output_messages: 236791
          output_updates: 2
          previous_connection_state: OpenConfirm
          received_prefix_count: 5
          remote_address: 192.168.0.4
          remote_as: 110
          remote_port: 179
          remove_private_as: false
          router_id: 192.179.0.73
          routing_table: inet.0
          suppress_4byte_as: false
          suppressed_prefix_count: 0
          up: true
  stderr: ''
  stdout: ''
```
napalm.get_lldp_neighbors
```
$ sudo st2 run napalm.get_lldp_neighbors hostname=ex4300-9
..
id: 59162094a374d80940a8c697
status: succeeded
parameters: 
  hostname: ex4300-9
result: 
  exit_code: 0
  result:
    raw:
      ge-0/0/0:
      - hostname: ex4300-17
        port: ex4300-9
      ge-0/0/1:
      - hostname: ex4300-18
        port: ex4300-9
  stderr: ''
  stdout: ''
```
napalm.get_interfaces
```
$ sudo st2 run napalm.get_interfaces -h

Get interfaces from a device using NAPALM.

Required Parameters:
    hostname
        The hostname of the device to connect to. Driver must be specified if
        hostname is not in configuration. Hostname without FQDN can be given
        if device is in configuration.
        Type: string

Optional Parameters:
    driver
        Device driver name for connecting to device, see
        https://napalm.readthedocs.io/en/latest/support/index.html for list.
        Type: string

    port
        port for accessing device
        Type: string

    credentials
        The credentials group which contains the username and password to log
        in
        Type: string

    interface
        Get details of a specific interface.
        Type: string

    counters
        Get only counter information for the interfaces interfaces
        Type: boolean

    ipaddresses
        Get only the ip addresses on the interfaces
        Type: boolean

    htmlout
        In addition to the normal output also includes html output.
        Type: boolean

    env
        Environment variables which will be available to the script.
        Type: object

    timeout
        Action timeout in seconds. Action will get killed if it doesn't finish
        in timeout seconds.
        Type: integer
        Default: 600
```
```
$ sudo st2 run napalm.get_interfaces hostname=ex4300-9 interface=ge-0/0/1
...
id: 5916c622a374d80940a8c6bb
status: succeeded
parameters: 
  hostname: ex4300-9
  interface: ge-0/0/1
result: 
  exit_code: 0
  result:
    raw:
      description: ex4300-18
      is_enabled: true
      is_up: true
      last_flapped: 11205279.0
      mac_address: 4C:96:14:E4:51:04
      name: ge-0/0/1
      speed: -1
  stderr: ''
  stdout: ''
```
```
$ sudo st2 run napalm.get_interfaces hostname=ex4300-9 interface=ge-0/0/1 counters=True
..
id: 5916c62ea374d80940a8c6be
status: succeeded
parameters: 
  counters: true
  hostname: ex4300-9
  interface: ge-0/0/1
result: 
  exit_code: 0
  result:
    raw:
      name: ge-0/0/1
      rx_broadcast_packets: 4589
      rx_discards: 0
      rx_errors: 0
      rx_multicast_packets: 356359
      rx_octets: 164697069
      rx_unicast_packets: 732777
      tx_broadcast_packets: 4483
      tx_discards: 0
      tx_errors: 0
      tx_multicast_packets: 356365
      tx_octets: 164465479
      tx_unicast_packets: 732891
  stderr: ''
  stdout: ''
```
```
$ sudo st2 run napalm.get_interfaces hostname=ex4300-9 ipaddresses=True
..
id: 5916c690a374d80940a8c6c1
status: succeeded
parameters: 
  hostname: ex4300-9
  ipaddresses: true
result: 
  exit_code: 0
  result:
    raw:
      bme0.0:
        ipv4:
          128.0.0.1:
            prefix_length: 2
          128.0.0.16:
            prefix_length: 2
          128.0.0.32:
            prefix_length: 2
          128.0.0.4:
            prefix_length: 2
      ge-0/0/0.0:
        ipv4:
          192.168.0.5:
            prefix_length: 31
      ge-0/0/1.0:
        ipv4:
          192.168.0.1:
            prefix_length: 31
      jsrv.1:
        ipv4:
          128.0.0.127:
            prefix_length: 2
      lo0.0:
        ipv4:
          127.0.0.1:
            prefix_length: 32
          192.179.0.95:
            prefix_length: 32
      me0.0:
        ipv4:
          172.30.179.95:
            prefix_length: 24
  stderr: ''
  stdout: ''
```
napalm.ping
```
$ sudo st2 run napalm.ping hostname=ex4300-9 destination=192.168.0.4 source=192.168.0.5 ttl=1
....
id: 5916c869a374d80940a8c6c4
status: succeeded
parameters: 
  destination: 192.168.0.4
  hostname: ex4300-9
  source: 192.168.0.5
  ttl: 1
result: 
  exit_code: 0
  result:
    raw:
      success:
        packet_loss: 0
        probes_sent: 5
        results:
        - ip_address: 192.168.0.4
          rtt: 3.589
        - ip_address: 192.168.0.4
          rtt: 1.195
        - ip_address: 192.168.0.4
          rtt: 3.148
        - ip_address: 192.168.0.4
          rtt: 3.154
        - ip_address: 192.168.0.4
          rtt: 2.165
        rtt_avg: 2.65
        rtt_max: 3.589
        rtt_min: 1.195
        rtt_stddev: 0.864
  stderr: ''
  stdout: ''
``` 


# ansible pack
## About Ansible
## About the ansible pack
## ansible pack installation
## ansible pack configuration file  
This pack doesnt use a configuration file. There is no configuration file for this pack in the directory `/opt/stackstorm/configs/`
## list of available actions with the integration pack ansible
## Execute actions from ansible pack using StackStorm CLI

# Executed actions

## Get the list of the 50 most recent action executions
```
$ sudo st2 execution list
+----------------------------+------------------+--------------+--------------------------+--------------------------+--------------------------+
| id                         | action.ref       | context.user | status                   | start_timestamp          | end_timestamp            |
+----------------------------+------------------+--------------+--------------------------+--------------------------+--------------------------+
|   5915f9a4a374d80940a8c679 | napalm.get_facts | st2admin     | succeeded (6s elapsed)   | Fri, 12 May 2017         | Fri, 12 May 2017         |
|                            |                  |              |                          | 18:06:28 UTC             | 18:06:34 UTC             |
|   5915f9bda374d80940a8c67c | napalm.get_facts | st2admin     | succeeded (6s elapsed)   | Fri, 12 May 2017         | Fri, 12 May 2017         |
|                            |                  |              |                          | 18:06:53 UTC             | 18:06:59 UTC             |
|   5915fa85a374d80940a8c67f | napalm.get_facts | st2admin     | succeeded (6s elapsed)   | Fri, 12 May 2017         | Fri, 12 May 2017         |
|                            |                  |              |                          | 18:10:13 UTC             | 18:10:19 UTC             |
|   5915fbb6a374d80940a8c682 | napalm.get_facts | st2admin     | succeeded (7s elapsed)   | Fri, 12 May 2017         | Fri, 12 May 2017         |
|                            |                  |              |                          | 18:15:18 UTC             | 18:15:25 UTC             |
+----------------------------+------------------+--------------+--------------------------+--------------------------+--------------------------+
```
## Get information about an executed action: 
```
$ sudo st2 execution get 5915fa85a374d80940a8c67f
id: 5915fa85a374d80940a8c67f
status: succeeded (6s elapsed)
parameters: 
  hostname: ex4300-17
result: 
  exit_code: 0
  result:
    raw:
      fqdn: ex4300-17.poc-nl.jnpr.net
      hostname: ex4300-17
      interface_list:
      - ge-0/0/0
      - pfe-0/0/0
      - pfh-0/0/0
      - ge-0/0/1
      - ge-0/0/2
      - ge-0/0/3
      - ge-0/0/4
      - ge-0/0/5
      - ge-0/0/6
      - ge-0/0/7
      - ge-0/0/8
      - ge-0/0/9
      - ge-0/0/10
      - ge-0/0/11
      - ge-0/0/12
      - ge-0/0/13
      - ge-0/0/14
      - ge-0/0/15
      - ge-0/0/16
      - ge-0/0/17
      - ge-0/0/18
      - ge-0/0/19
      - ge-0/0/20
      - ge-0/0/21
      - ge-0/0/22
      - ge-0/0/23
      - .local.
      - bme0
      - dsc
      - gre
      - ipip
      - irb
      - jsrv
      - lo0
      - lsi
      - me0
      - mtun
      - pimd
      - pime
      - tap
      - vme
      model: EX4300-24T
      os_version: 15.1R5.5
      serial_number: PG3713430170
      uptime: 11147520
      vendor: Juniper
  stderr: ''
  stdout: ''
```

# Looking for more Junos automation examples:  

Junos automation with Network Infrastructure as Code 
https://gitlab.com/ksator/network-infrastructure-as-code  

How to automate Openconfig configuration on Juniper devices  
https://github.com/ksator/openconfig-demo-with-juniper-devices  

How to automate verifications on Junos devices with JSNAPy
https://github.com/ksator/junos-verifications-automation-with-jsnapy  

How to delegate junos automation tasks chatting to a bot with slack   
https://github.com/ksator/junos-automation-with-chatops-in-ams  

How to automate junos with ansible  
https://github.com/ksator/ansible-training-for-junos-automation  

How to automate junos with python 
https://github.com/ksator/python-training-for-network-engineers  

How to orchestrate Junos virtual machines with Vagrant 
https://github.com/ksator/vagrant-with-junos  


