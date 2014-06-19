# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
    config.vm.box = 'azure'

    config.vm.provider :azure do |azure|
        azure.mgmt_certificate = '' # create in step 6
        azure.mgmt_endpoint = 'https://management.core.windows.net'
        azure.subscription_id = '' # from step 3
        azure.vm_image = 'b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04-LTS-amd64-server-20140528-en-us-30GB' # from step 4
        azure.vm_name = 'vagrant-test' # max 15 characters. contains letters, number and hyphens. can start with letters and can end with letters and numbers
        azure.vm_location = '' # from step 5. e.g., West US
        azure.ssh_private_key_file = '' # your private key typically in ~/.ssh/id_rsa
        azure.ssh_certificate_file = '' # your X.509 key from step 9
        
        #azure.storage_acct_name = 'NAME OF YOUR STORAGE ACCOUNT' # optional. A new one will be generated if not provided.
        #azure.vm_user = 'PROVIDE A USERNAME' # defaults to 'vagrant' if not provided
        #azure.vm_password = 'PROVIDE A VALID PASSWORD' # min 8 characters. should contain a lower case letter, an uppercase letter, a number and a special character
        #azure.cloud_service_name = 'PROVIDE A NAME FOR YOUR CLOUD SERVICE' # same as vm_name. leave blank to auto-generate
        #azure.deployment_name = 'PROVIDE A NAME FOR YOUR DEPLOYMENT' # defaults to cloud_service_name
        #azure.ssh_port = '22' # if you don't want to use the standard port 

        # Provide the following values if creating a Windows VM
        #azure.winrm_transport = [ 'http', 'https' ] # this will open up winrm ports on both http (5985) and http (5986) ports
        #azure.winrm_https_port = 'A VALID PUBLIC PORT' # customize the winrm https port, instead of 5986
        #azure.winrm_http_port = 'A VALID PUBLIC PORT' # customize the winrm http port, insted of 5985

        #azure.tcp_endpoints = '3389:53389' # opens the Remote Desktop internal port that listens on public port 53389. Without this, you cannot RDP to a Windows VM.
    end

    config.ssh.username = 'vagrant' # the one used to create the VM
    #config.ssh.password = 'vagrant' # If windows or user/pass auth enabled
    config.ssh.private_key_path = '/Users/Phil/.ssh/id_rsa' # Your own private key. Don't use the Vagrant insecure private key
end

