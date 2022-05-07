Step:

1. Supported SSH key formats
    
2. Create an SSH key pair
        The following command creates an SSH key pair using RSA encryption and a bit length of 4096:`ssh-keygen -m PEM -t rsa -b 4096`
3. Provide an SSH public key when deploying a VM
        you can display your public key with the following cat command, replacing ~/.ssh/id_rsa.pub with the path and filename of your own public key file if needed: `cat ~/.ssh/id_rsa.pub`


4. SSH into your VM
    `ssh azureuser@myvm.australaeast.cloudapp.azure.com`
     used public IP :`ssh azureuser@20.213.25.146`