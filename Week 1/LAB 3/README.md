# Lab 3: Manage Azure disks with the Azure CLI

Task:

1. Default Azure disks
2. Azure data disks
3. VM disk types
4. Launch Azure Cloud Shell
5. Create and attach disks

After launching cloud shell I create a resource group called myResourceGroupDisk in australiaeast

damilola@Azure:~$ az group create --name myResourceGroupDisk --location australiaeast
{
  "id": "/subscriptions/4806a9a7-e202-49ac-856e-ec8fa95a2c39/resourceGroups/myResourceGroupDisk",
  "location": "australiaeast",
  "managedBy": null,
  "name": "myResourceGroupDisk",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"

  I Attached disk at VM creation specifying disk size


  damilola@Azure:~$ az vm create \
>   --resource-group myResourceGroupDisk \
>   --name myVM \
>   --image UbuntuLTS \
>   --size Standard_DS2_v2 \
>   --admin-username azureuser \
>   --generate-ssh-keys \
>   --data-disk-sizes-gb 128 128

{
  "fqdns": "",
  "id": "/subscriptions/4806a9a7-e202-49ac-856e-ec8fa95a2c39/resourceGroups/myResourceGroupDisk/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "australiaeast",
  "macAddress": "00-22-48-11-70-4F",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "20.53.224.122",
  "resourceGroup": "myResourceGroupDisk",
  "zones": ""
}

I Attached another disk to existing VM

damilola@Azure:~$ az vm disk attach \
>     --resource-group myResourceGroupDisk \
>     --vm-name myVM \
>     --name myDataDisk \
>     --size-gb 128 \
>     --sku Premium_LRS \
>     --new
6. Prepare data disks
Create an SSH connection with the virtual machine

damilola@Azure:~$ ssh azureuser@20.53.224.122
The authenticity of host '20.53.224.122 (20.53.224.122)' can't be established.
ECDSA key fingerprint is SHA256:yLlIR1FlhGVYrl+l+bYt5DdsKWPTs1zDMof4Zg4zfFg.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '20.53.224.122' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 5.4.0-1077-azure x86_64)

Partition the disk with parted.



azureuser@myVM:~$ sudo parted /dev/sdc --script mklabel gpt mkpart xfspart xfs 0% 100%

Write a file system to the partition by using the mkfs command. Use partprobe to make the OS aware of the change.

azureuser@myVM:~$ sudo mkfs.xfs /dev/sdc1
meta-data=/dev/sdc1              isize=512    agcount=4, agsize=8388480 blks
         =                       sectsz=4096  attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=0, rmapbt=0, reflink=0
data     =                       bsize=4096   blocks=33553920, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=16383, version=2
         =                       sectsz=4096  sunit=1 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0

Mount the new disk so that it is accessible in the operating system.

azureuser@myVM:~$ df -h | grep -i "sd"
/dev/sda1        29G  1.5G   28G   5% /
/dev/sda15      105M  4.4M  100M   5% /boot/efi
/dev/sdd1        14G   41M   13G   1% /mnt
/dev/sdc1       128G  163M  128G   1% /datadrive

To ensure that the drive is remounted after a reboot, it must be added to the /etc/fstab file. To do so, get the UUID of the disk with the blkid utility.

azureuser@myVM:~$ sudo -i blkid
/dev/sda1: LABEL="cloudimg-rootfs" UUID="f9fdb886-652e-49ff-91b5-4d4de3470558" TYPE="ext4" PARTUUID="ca07caf0-b05e-4bec-bc6c-a0f21b274ea9"
/dev/sda15: LABEL="UEFI" UUID="ABE4-D506" TYPE="vfat" PARTUUID="33e7de82-6c5b-4dbc-9f65-b0a0a661a29f"
/dev/sdd1: UUID="66a4b36a-0872-4713-a400-1a206b9a65bd" TYPE="ext4" PARTUUID="bce26375-01"
/dev/sda14: PARTUUID="a6e40093-1ebf-4a3e-98ea-474e6705c321"
/dev/sdc1: UUID="3b814313-b04a-42de-9861-2f71fcc9cabc" TYPE="xfs" PARTLABEL="xfspart" PARTUUID="1b1bda52-39a0-46e5-83ad-3559d6c8b79e"

The output displays the UUID of the drive, /dev/sdc1 in this case.




7. Take a disk snapshot


Notes:
Quickstart: Manage Azure disks

https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-manage-disks
Quickstart for Bash in Azure Cloud Shell

https://docs.microsoft.com/en-us/azure/cloud-shell/quickstart