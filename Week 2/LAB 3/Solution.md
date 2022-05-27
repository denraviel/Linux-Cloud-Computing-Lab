1. Create a Linux VM

    I created a resource group called schull with command:`az group create --name schulltech --location australiaeast`
    
    Created a VM: `az vm create -name schulltech -resource-group schulltech --admin-username your-user-name --image Ubuntu LTS --generate-ssh-keys`
