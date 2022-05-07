Steps: 

1. Create resource group
  Created a resource group as done in lab1 : `az group create --name myResourceGroupVM --location australiaeast` 
2. Create virtual machine
  Created a virtual machine called myVm: `az vm create -name --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --admin-username azureuser --generate-ssh-keys`
3. Connect to VM
  Connected to VM using public ip address: `ssh azureuser@20.92.80.17`

4. Understand VM images
  The Azure marketplace includes many images that can be used to create VMs.
  To see a list of the most commonly used images, use the az vm image list command:`az vm image list --offer CentOS --all --output table`.
    Some of the Vm images on azure are :CentOS from open logic ,debian-10 from Debian,RHEL from redhat.

  we can use version numbers to specify what version number for the image we want to use.
  We can also replace version number with "latest" , which selects the latest version of the distribution :`az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:7.5:latest --generate-ssh-keys`



5. Understand VM sizes
    The quantity of processing resources made accessible to a virtual machine, such as CPU, GPU, and RAM, is determined by its size. Virtual machines must be properly sized for the anticipated workload. An existing virtual machine can be resized if the workload grows.
  
  To see a list of VM sizes available in a particular region, use the az vm list-sizes command: `az vm list-sizes --location australiaeast --output table`

  A VM size can be selected at creation time using az vm create and the --size argument:`az vm create \ --resource-group myResourceGroupVM \ --name myVM3 \ --image UbuntuLTS \  --size Standard_B1s \  --generate-ssh-keys`

  A VM can be resized after it has been launched to increase or decrease resource allocation.

  You can view the current of size of a VM with az vm show:`az vm show --resource-group myResourceGroupVM --name myVM --query hardwareProfile.vmSize`. The size of my virtual machine(myVM) in hands on is "Standard_DS1_v2"

 Check if the desired size is available on the current Azure cluster before resizing a VM. The command az vm list-vm-resize-options returns a list of sizes.:`az vm list-vm-resize-options --resource-group myResourceGroupVM --name myVM --query [].name`

 The resize is done with the az vm resize command:`az vm resize --resource-group myResourceGroupVM --name myVM --size standard_b1ms`

 If the intended size is not available on the present cluster, the VM must be deallocated before the resize can be performed. Stop and deallocate the VM with the az vm deallocate command. Any data on the transient disk may be removed when the VM is powered back on. Unless a static IP address is utilized, the public IP address also changes:`az vm deallocate --resource-group myResourceGroupVM --name myVM`

We can now resize:`az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_GS1`

After the resize, the VM can be started:`az vm start --resource-group myResourceGroupVM --name myVM`

6. VM power states
    An Azure VM can have one of many power states;"Starting","Running","Stopping","Stopped",
    "Deallocating","Deallocated"
    
    Use the az vm get-instance-view command to acquire the state of a specific VM. Make that a virtual machine and resource group have valid names:`az vm get-instance-view \ --name myVM \ --resource-group myResourceGroupVM \  --query instanceView.statuses[1] --output table`

7. Management tasks
This command returns a virtual machine's private and public IP addresses: `az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table`


Stop virtual machine:`az vm stop --resource-group myResourceGroupVM --name myVM`


Start virtual machine:` az vm start --resource-group myResourceGroupVM --name myVM`


Deleting VM resources:`az group delete --name myResourceGroupVM --no-wait --yes`
