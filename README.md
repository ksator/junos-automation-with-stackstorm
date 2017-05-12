## about this repo
How to automate Junos with stackstorm. 

## about stackstorm
StackStorm is event-driven automation: 
- it doesnt rely on human input. 
- it reacts to events.  

Stackstorm has network automation capabilities with the stackstorm packs napalm and ansible. So stackstorm can fire napalm or ansible content based on events.    

https://keepingitclassless.net/2016/12/introduction-to-stackstorm/ (Matt Oswalt)  
https://medium.com/netflix-techblog/introducing-winston-event-driven-diagnostic-and-remediation-platform-46ce39aa81cc (netflix)  


## stackstorm documentation: 

stackstorm documentation: https://docs.stackstorm.com/index.html  
stackstorm exchange (integration packs): https://exchange.stackstorm.org/  
github: https://github.com/StackStorm  
slack team: https://stackstorm-community.slack.com  

## stackstorm installation: 

### all-in-one quick installation: 
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

### Verify: 
```
$ st2 --version
st2 2.2.1
$ st2 --help
```
### User: 
```
$ whoami
ksator
$ st2 whoami 
Currently logged in as st2admin
```


## Looking for more Junos automation examples:  

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

How to orchestrate your Junos virtual machines with Vagrant (vagrant, virtual box, ansible, vsrx, vqfx)  
https://github.com/ksator/vagrant-with-junos  


