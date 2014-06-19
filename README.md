vagrant-azure-example
=====================

I found the documentation of how to get the vagrant-azure plugin working quite lacking, so here are some more complete instructions for those out there who might be starting out with Vagrant and Azure who aren't running Windows as their desktop.

This project was developed on OSX Mavericks, creating an Ubuntu Trusty Tahr 14.04 LTS VM on Azure.

Requirements
------------
* Vagrant
* Vagrant-azure plugin
* Microsoft Azure account 
* Node package manager
* Azure Command line tools
* Various X.509 certs and keys that Azure requires

Install Dependencies
-------------------
#### Install Vagrant
Download and install vagrant from [here](http://www.vagrantup.com/downloads.html)
#### Install the vagrant-azure plugin
Install the plugin with the command `vagrant plugin install vagrant-azure`
#### Create Azure account
If you don't already have an Azure account you can sign up for a trial [here](http://azure.microsoft.com/)
#### Install npm
If you're using homebrew you can just perform `brew install node`. Alternatively you can install node using [these directions](http://coolestguidesontheplanet.com/installing-node-js-osx-10-9-mavericks/). You really only need this for the Node package manager, npm.
#### Install Azure command line tools
Install the Azure CLI tools `npm install azure-cli -g`. The full list of available commands can be viewed [here](http://azure.microsoft.com/en-us/documentation/articles/command-line-tools/).

Setup Instructions
-------------------
Here are the directions to setup the Azure CLI tools, generate the necessary keys and find out the correct information about the Azure environment that you need for you Vagrantfile. You should perform all of the commands below.
<ol>
<li><code>azure account download</code> Download your account details. This comes in a file and contains sentitive information. Keep it safe.</li>
<li><code>azure account import publishsettings.publishsettings</code> Import your account details from the file you just downloaded.</li>
<li><code>azure account list</code> Note down the ID of the account where you want to create your vagrant VM.</li>
<li><code>azure vm image list</code> Note down the name of the base image from which you'd like to create your VM.</li> 
</ol>
Thanks to Brett Swift for detailing the [next instructions](https://github.com/MSOpenTech/vagrant-azure/issues/10). 
<ol start="4">
<li><code>openssl req -x509 -nodes -days 3650  -newkey rsa:2048 -keyout cert.pem -out cert.pem</code> Create an X.509 certificate to authenticate with Azure. You can leave all the identification information blank if you wish. Azure doesn't care.</li>
<li><code>openssl pkcs12 -export -out cert.pfx -in cert.pem -name "My Cert"</code> Create a pkcs12 archive (~Microsoft Parallel FX) from the X.509 cert. Enter an encryption password if you wish.</li>
<li><code>openssl x509 -inform pem -in cert.pem -outform der -out cert.cer</code> Create .cer certificate to upload to Azure.</li>
</ol>
Log in to the Azure management console, go the to settings page in the management portal and click on MANAGEMENT CERTIFICATES and upload the .cer file created above. Make sure you add the certificate to the correct subscription ID. See [here](http://msdn.microsoft.com/library/azure/gg551722.aspx) for more detail.

I'll assume you already have an RSA pub/private ssh key to log into *nix machines. You may not have an X.509 key that Azure requires however. You could use the one you just created, however you may prefer to create one from your existing RSA private key so that you can log in with your normal credentials.
<ol start="8">
<li><code>openssl req -new -x509 -key ~/.ssh/id_rsa -out ~/.ssh/ssh-cert.pem</code></li>
</ol>

Download this dummy box that vagrant need to use the azure provider.
<ol start="9">
<li><code>vagrant box add azure https://github.com/msopentech/vagrant-azure/raw/master/dummy.box</code></li>
</ol>

Creating the box
-------------------
Finally you're ready to start creating your Vagrantfile. Clone this repo and update the vagrant file with the necessary details from the instructions above.

| Parameter | Explanation |
|------------------------|------------------------|
| azure.mgmt_certificate | created in step step 5 |
| azure.subscription_id | from step 3 |
| azure.vm_image | from step 4 |
| azure.ssh_private_key_file | your private rsa key for the vagrant-azure plugin |
| azure.ssh_certificate_file | from step 8 |
| config.ssh.private_key_path | your private rsa key for vagrant |

Now you should be ready to create your vagrant box on Azure.

`vagrant up --provider=azure`
