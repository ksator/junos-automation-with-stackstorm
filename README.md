## about this repo
How to automate Junos with stackstorm (using stackstorm packs napalm and ansible). 

## about stackstorm
StackStorm is event-driven automation: 
- doesnt rely on human input. 
- react to events.
https://keepingitclassless.net/2016/12/introduction-to-stackstorm/ (Matt Oswalt)  
https://medium.com/netflix-techblog/introducing-winston-event-driven-diagnostic-and-remediation-platform-46ce39aa81cc (netflix)

## stackstorm documentation: 
https://docs.stackstorm.com/index.html

## slack: 
https://stackstorm-community.slack.com

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
