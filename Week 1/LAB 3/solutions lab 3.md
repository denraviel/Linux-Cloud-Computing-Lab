Steps:

1. Default Azure disks
    Disks are used to store the operating system, programs, and data in Azure virtual machines (VMs). When creating a virtual machine, it's critical to select a disk size and configuration that's appropriate for the predicted workload.

    When you build a virtual machine in Azure, two disks are immediately associated to it.
    
    i. Operating system disks - These drives can hold up to 2 TB of data and house the VM's operating system.The OS disk should not be used for apps or data because of this arrangement.

    ii. Temporary disks use a solid-state drive that is on the same Azure host as the virtual machine. Temp disks are extremely fast and can be utilized for tasks like temporary data processing. Any data stored on a temporary disk is removed if the VM is relocated to a new host. The VM size determines the size of the temporary disk. /dev/sdb and /mnt are the mountpoints for temporary disks.

2. Azure data disks
    Additional data disks can be installed to install software and store data. In any circumstance where long-term data storage is required, data disks should be employed. The number of data disks that can be attached to a virtual machine is determined by its size.
    

3. VM disk types

    There are two types of disks available in Azure.

  i. Standard disks are supported by HDDs and provide cost-effective storage while   maintaining performance.

  ii. Premium disks are backed by high-performance, low-latency SSD-based disks. Perfect for production workloads in virtual machines.
4. Launch Azure Cloud Shell
5. Create and attach disks
    Create a resource group with the az group create command:`az group create --name myResourceGroupDisk --location australiaeast`

    I created a VM with an UbuntuLTS image with admin username azureuser an extra disk was created and attached to the virtual machine using the —datadisk-sizes-gb parameter.In the example below, a VM is constructed with two 128 GB data disks:`az vm create \ --resource-group myResourceGroupDisk \  --name myVM \ --image UbuntuLTS \ --size Standard_DS2_v2 \ --admin-username azureuser \ --generate-ssh-keys \ --data-disk-sizes-gb 128 128`

    Used the az vm disk attach command to create and attach a new disk to an existing virtual machine. The following example generates a 128-gigabyte premium drive and joins it to the virtual machine created in the previous step:`az vm disk attach \ --resource-group myResourceGroupDisk \ --vm-name myVM \ --name myDataDisk \ --size-gb 128 \ --sku Premium_LRS \ --new`

   Connected to the virtual computer using SSH. Substitute the virtual machine's public IP address for the example IP address:`ssh azureuser@20.213.131.140`

   Partition the disk with parted:`sudo parted /dev/sdc --script mklabel gpt mkpart xfspart xfs 0% 100%`

   Using the mkfs command, created a file system on the partition. To notify the OS of the change, use partprobe:`sudo mkfs.xfs /dev/sdc1` `sudo partprobe /dev/sdc1`

   Mounted the new disk so that it is accessible in the operating system:`sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive`

   The disk is now accessible via the /datadrive mountpoint, which may be verified with the df -h command:`df -h | grep -i "sd"`

   To ensure that the drive is remounted after a reboot, it must be added to the /etc/fstab file. To do so, get the UUID of the disk with the blkid utility:`sudo -i blkid`
   
   Open the /etc/fstab file in a text editor as follows:`sudo nano /etc/fstab`

   After writing to the file with `Ctrl+O` ,I used `Ctrl+X` to exit the editor

7. Take a disk snapshot

    Before you create a snapshot, you need the ID or name of the disk. Use az vm show to shot the disk ID

  This snapshot can then be converted into a disk using az disk create, which can be used to recreate the virtual machine.
