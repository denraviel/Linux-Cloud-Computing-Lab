1. Launch Azure Cloud Shell
  I lauched the cloud shell on microsoft azure console

2. Create a resource group
  I created a resource group called myResourceGroup with the command: `az group create --name MyResourceGroup --location australiaeast`

3. Create virtual machine
  In the Resource group I created a virtual machine called myVM with an UbuntuLTS image  :`az vm create -name --resource-group myResourceGroup --name myVM --image UbuntuLTS  --admin-username azureuser --generate-ssh-keys`
4. Open port 80 for web traffic

  I Used az vm open-port to open TCP port 80 for use with the NGINX web server
  :`az vm open-port --port 80 --resource-group myResourceGroup --name myVM`
5. Connect to virtual machine

  I connected to my vm using ssh and my vm's public address: `ssh azureuser@20.211.34.176`

  confirmed connection by typing in yes
  connected successfully
 6. Install web server
  I updated packages and installed nginx web server :`sudo apt-get -y update` and `sudo apt-get -y install nginx`