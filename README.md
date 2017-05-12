## stackstorm documentation
https://docs.stackstorm.com/index.html

## slack
https://stackstorm-community.slack.com

## stackstorm installation: 
To quickly install ST2 on ubuntu (single-box deployment), run these commands: 
```
sudo apt-get install curl
curl -sSL https://stackstorm.com/packages/install.sh | bash -s -- --user=st2admin --password=Ch@ngeMe
```
The installation takes about 12 minutes. Once it completes successfully, you will see the following output:
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

Verify
```
$ st2 --version
st2 2.2.1
$ st2 whoami 
Currently logged in as st2admin
$ whoami
ksator
$ st2 --help
```
