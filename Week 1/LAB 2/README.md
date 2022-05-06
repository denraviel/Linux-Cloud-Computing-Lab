# Lab 2: Manage Linux VMs with the Azure CLI

Task: 

1. Create resource group
damilola@Azure:~$ az group create --name myResourceGroupVM --location australiaeast
{
  "id": "/subscriptions/4806a9a7-e202-49ac-856e-ec8fa95a2c39/resourceGroups/myResourceGroupVM",
  "location": "australiaeast",
  "managedBy": null,
  "name": "myResourceGroupVM",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": "Microsoft.Resources/resourceGroups"
}
2. Create virtual machine
damilola@Azure:~$ az vm create \
>     --resource-group myResourceGroupVM \
>     --name myVM \
>     --image UbuntuLTS \
>     --admin-username azureuser \
>     --generate-ssh-keys

{
  "fqdns": "",
  "id": "/subscriptions/4806a9a7-e202-49ac-856e-ec8fa95a2c39/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "australiaeast",
  "macAddress": "00-0D-3A-D1-F1-F4",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "20.211.161.18",
  "resourceGroup": "myResourceGroupVM",
  "zones": ""
}
3. Connect to VM
damilola@Azure:~$ ssh azureuser@20.211.161.18
The authenticity of host '20.211.161.18 (20.211.161.18)' can't be established.
ECDSA key fingerprint is SHA256:p3X8EJGKVvtBPdAPKyfo0kPi9yhTecShs4+sEjM1GOc.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '20.211.161.18' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 5.4.0-1077-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri May  6 13:46:12 UTC 2022

  System load:  0.07              Processes:           114
  Usage of /:   4.8% of 28.90GB   Users logged in:     0
  Memory usage: 5%                IP address for eth0: 10.0.0.4
  Swap usage:   0%

0 updates can be applied immediately.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

4. Understand VM images
damilola@Azure:~$ az vm image list --output table
You are viewing an offline list of images, use --all to retrieve an up-to-date list
Offer                         Publisher               Sku                 Urn                                                             UrnAlias             Version
----------------------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
CentOS                        OpenLogic               7.5                 OpenLogic:CentOS:7.5:latest                                     CentOS               latest
debian-10                     Debian                  10                  Debian:debian-10:10:latest                                      Debian               latest
flatcar-container-linux-free  kinvolk                 stable              kinvolk:flatcar-container-linux-free:stable:latest              Flatcar              latest
opensuse-leap-15-3            SUSE                    gen2                SUSE:opensuse-leap-15-3:gen2:latest                             openSUSE-Leap        latest
RHEL                          RedHat                  7-LVM               RedHat:RHEL:7-LVM:latest                                        RHEL                 latest
sles-15-sp3                   SUSE                    gen2                SUSE:sles-15-sp3:gen2:latest                                    SLES                 latest
UbuntuServer                  Canonical               18.04-LTS           Canonical:UbuntuServer:18.04-LTS:latest                         UbuntuLTS            latest
WindowsServer                 MicrosoftWindowsServer  2022-Datacenter     MicrosoftWindowsServer:WindowsServer:2022-Datacenter:latest     Win2022Datacenter    latest
WindowsServer                 MicrosoftWindowsServer  2019-Datacenter     MicrosoftWindowsServer:WindowsServer:2019-Datacenter:latest     Win2019Datacenter    latest
WindowsServer                 MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer                 MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer                 MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
WindowsServer                 MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest



damilola@Azure:~$ az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:7.5:latest --generate-ssh-keys
It is recommended to use parameter "--public-ip-sku Standard" to create new VM with Standard public IP. Please note that the default public IP used for VM creation will be changed from Basic to Standard in the future.
{
  "fqdns": "",
  "id": "/subscriptions/4806a9a7-e202-49ac-856e-ec8fa95a2c39/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM2",
  "location": "australiaeast",
  "macAddress": "00-22-48-12-13-06",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "20.213.85.115",
  "resourceGroup": "myResourceGroupVM",
  "zones": ""
}

5. Understand VM sizes
damilola@Azure:~$ az vm list-sizes --location australiaeast --output table
MaxDataDiskCount    MemoryInMb    Name                    NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
4                   8192          Standard_D2ds_v4        2                1047552           76800
8                   16384         Standard_D4ds_v4        4                1047552           153600
16                  32768         Standard_D8ds_v4        8                1047552           307200
32                  65536         Standard_D16ds_v4       16               1047552           614400
32                  131072        Standard_D32ds_v4       32               1047552           1228800
32                  196608        Standard_D48ds_v4       48               1047552           1843200
32                  262144        Standard_D64ds_v4       64               1047552           2457600
4                   8192          Standard_D2ds_v5        2                1047552           76800
8                   16384         Standard_D4ds_v5        4                1047552           153600
16                  32768         Standard_D8ds_v5        8                1047552           307200
32                  65536         Standard_D16ds_v5       16               1047552           614400
32                  131072        Standard_D32ds_v5       32               1047552           1228800
32                  196608        Standard_D48ds_v5       48               1047552           1843200
32                  262144        Standard_D64ds_v5       64               1047552           2457600
32                  393216        Standard_D96ds_v5       96               1047552           3686400
4                   8192          Standard_D2d_v4         2                1047552           76800
8                   16384         Standard_D4d_v4         4                1047552           153600
16                  32768         Standard_D8d_v4         8                1047552           307200
32                  65536         Standard_D16d_v4        16               1047552           614400
32                  131072        Standard_D32d_v4        32               1047552           1228800
32                  196608        Standard_D48d_v4        48               1047552           1843200
32                  262144        Standard_D64d_v4        64               1047552           2457600
4                   8192          Standard_D2d_v5         2                1047552           76800
8                   16384         Standard_D4d_v5         4                1047552           153600
16                  32768         Standard_D8d_v5         8                1047552           307200
32                  65536         Standard_D16d_v5        16               1047552           614400
32                  131072        Standard_D32d_v5        32               1047552           1228800
32                  196608        Standard_D48d_v5        48               1047552           1843200
32                  262144        Standard_D64d_v5        64               1047552           2457600
32                  393216        Standard_D96d_v5        96               1047552           3686400
4                   3584          Standard_DS1_v2         1                1047552           7168
8                   7168          Standard_DS2_v2         2                1047552           14336
16                  14336         Standard_DS3_v2         4                1047552           28672
32                  28672         Standard_DS4_v2         8                1047552           57344
64                  57344         Standard_DS5_v2         16               1047552           114688
8                   14336         Standard_DS11-1_v2      2                1047552           28672
8                   14336         Standard_DS11_v2        2                1047552           28672
16                  28672         Standard_DS12-1_v2      4                1047552           57344
16                  28672         Standard_DS12-2_v2      4                1047552           57344
16                  28672         Standard_DS12_v2        4                1047552           57344
32                  57344         Standard_DS13-2_v2      8                1047552           114688
32                  57344         Standard_DS13-4_v2      8                1047552           114688
32                  57344         Standard_DS13_v2        8                1047552           114688
64                  114688        Standard_DS14-4_v2      16               1047552           229376
64                  114688        Standard_DS14-8_v2      16               1047552           229376
64                  114688        Standard_DS14_v2        16               1047552           229376
64                  143360        Standard_DS15_v2        20               1047552           286720
8                   7168          Standard_DS2_v2_Promo   2                1047552           14336
16                  14336         Standard_DS3_v2_Promo   4                1047552           28672
32                  28672         Standard_DS4_v2_Promo   8                1047552           57344
64                  57344         Standard_DS5_v2_Promo   16               1047552           114688
8                   14336         Standard_DS11_v2_Promo  2                1047552           28672
16                  28672         Standard_DS12_v2_Promo  4                1047552           57344
32                  57344         Standard_DS13_v2_Promo  8                1047552           114688
64                  114688        Standard_DS14_v2_Promo  16               1047552           229376
4                   8192          Standard_D2s_v3         2                1047552           16384
8                   16384         Standard_D4s_v3         4                1047552           32768
16                  32768         Standard_D8s_v3         8                1047552           65536
32                  65536         Standard_D16s_v3        16               1047552           131072
32                  131072        Standard_D32s_v3        32               1047552           262144
32                  196608        Standard_D48s_v3        48               1047552           393216
32                  262144        Standard_D64s_v3        64               1047552           524288
4                   8192          Standard_D2s_v4         2                1047552           0
8                   16384         Standard_D4s_v4         4                1047552           0
16                  32768         Standard_D8s_v4         8                1047552           0
32                  65536         Standard_D16s_v4        16               1047552           0
32                  131072        Standard_D32s_v4        32               1047552           0
32                  196608        Standard_D48s_v4        48               1047552           0
32                  262144        Standard_D64s_v4        64               1047552           0
4                   8192          Standard_D2s_v5         2                1047552           0
8                   16384         Standard_D4s_v5         4                1047552           0
16                  32768         Standard_D8s_v5         8                1047552           0
32                  65536         Standard_D16s_v5        16               1047552           0
32                  131072        Standard_D32s_v5        32               1047552           0
32                  196608        Standard_D48s_v5        48               1047552           0
32                  262144        Standard_D64s_v5        64               1047552           0
32                  393216        Standard_D96s_v5        96               1047552           0
4                   3584          Standard_D1_v2          1                1047552           51200
8                   7168          Standard_D2_v2          2                1047552           102400
16                  14336         Standard_D3_v2          4                1047552           204800
32                  28672         Standard_D4_v2          8                1047552           409600
64                  57344         Standard_D5_v2          16               1047552           819200
8                   14336         Standard_D11_v2         2                1047552           102400
16                  28672         Standard_D12_v2         4                1047552           204800
32                  57344         Standard_D13_v2         8                1047552           409600
64                  114688        Standard_D14_v2         16               1047552           819200
64                  143360        Standard_D15_v2         20               1047552           1024000
8                   7168          Standard_D2_v2_Promo    2                1047552           102400
16                  14336         Standard_D3_v2_Promo    4                1047552           204800
32                  28672         Standard_D4_v2_Promo    8                1047552           409600
64                  57344         Standard_D5_v2_Promo    16               1047552           819200
8                   14336         Standard_D11_v2_Promo   2                1047552           102400
16                  28672         Standard_D12_v2_Promo   4                1047552           204800
32                  57344         Standard_D13_v2_Promo   8                1047552           409600
64                  114688        Standard_D14_v2_Promo   16               1047552           819200
4                   8192          Standard_D2_v3          2                1047552           51200
8                   16384         Standard_D4_v3          4                1047552           102400
16                  32768         Standard_D8_v3          8                1047552           204800
32                  65536         Standard_D16_v3         16               1047552           409600
32                  131072        Standard_D32_v3         32               1047552           819200
32                  196608        Standard_D48_v3         48               1047552           1228800
32                  262144        Standard_D64_v3         64               1047552           1638400
4                   8192          Standard_D2_v4          2                1047552           0
8                   16384         Standard_D4_v4          4                1047552           0
16                  32768         Standard_D8_v4          8                1047552           0
32                  65536         Standard_D16_v4         16               1047552           0
32                  131072        Standard_D32_v4         32               1047552           0
32                  196608        Standard_D48_v4         48               1047552           0
32                  262144        Standard_D64_v4         64               1047552           0
4                   8192          Standard_D2_v5          2                1047552           0
8                   16384         Standard_D4_v5          4                1047552           0
16                  32768         Standard_D8_v5          8                1047552           0
32                  65536         Standard_D16_v5         16               1047552           0
32                  131072        Standard_D32_v5         32               1047552           0
32                  196608        Standard_D48_v5         48               1047552           0
32                  262144        Standard_D64_v5         64               1047552           0
32                  393216        Standard_D96_v5         96               1047552           0
4                   16384         Standard_E2ds_v4        2                1047552           76800
8                   32768         Standard_E4-2ds_v4      4                1047552           153600
8                   32768         Standard_E4ds_v4        4                1047552           153600
16                  65536         Standard_E8-2ds_v4      8                1047552           307200
16                  65536         Standard_E8-4ds_v4      8                1047552           307200
16                  65536         Standard_E8ds_v4        8                1047552           307200
32                  131072        Standard_E16-4ds_v4     16               1047552           614400
32                  131072        Standard_E16-8ds_v4     16               1047552           614400
32                  131072        Standard_E16ds_v4       16               1047552           614400
32                  163840        Standard_E20ds_v4       20               1047552           768000
32                  262144        Standard_E32-8ds_v4     32               1047552           1228800
32                  262144        Standard_E32-16ds_v4    32               1047552           1228800
32                  262144        Standard_E32ds_v4       32               1047552           1228800
32                  393216        Standard_E48ds_v4       48               1047552           1843200
32                  516096        Standard_E64-16ds_v4    64               1047552           2457600
32                  516096        Standard_E64-32ds_v4    64               1047552           2457600
32                  516096        Standard_E64ds_v4       64               1047552           2457600
4                   16384         Standard_E2ds_v5        2                1047552           76800
8                   32768         Standard_E4-2ds_v5      4                1047552           153600
8                   32768         Standard_E4ds_v5        4                1047552           153600
16                  65536         Standard_E8-2ds_v5      8                1047552           307200
32                  65536         Standard_E8-4ds_v5      8                1047552           307200
16                  65536         Standard_E8ds_v5        8                1047552           307200
32                  131072        Standard_E16-4ds_v5     16               1047552           614400
32                  131072        Standard_E16-8ds_v5     16               1047552           614400
32                  131072        Standard_E16ds_v5       16               1047552           614400
32                  163840        Standard_E20ds_v5       20               1047552           768000
32                  262144        Standard_E32-8ds_v5     32               1047552           1228800
32                  262144        Standard_E32-16ds_v5    32               1047552           1228800
32                  262144        Standard_E32ds_v5       32               1047552           1228800
32                  393216        Standard_E48ds_v5       48               1047552           1843200
32                  524288        Standard_E64-16ds_v5    64               1047552           2457600
32                  524288        Standard_E64-32ds_v5    64               1047552           2457600
32                  524288        Standard_E64ds_v5       64               1047552           2457600
32                  688128        Standard_E96-24ds_v5    96               1047552           2457600
32                  688128        Standard_E96-48ds_v5    96               1047552           2457600
32                  688128        Standard_E96ds_v5       96               1047552           3686400
64                  688128        Standard_E104ids_v5     104              1047552           3891200
4                   16384         Standard_E2d_v4         2                1047552           76800
8                   32768         Standard_E4d_v4         4                1047552           153600
16                  65536         Standard_E8d_v4         8                1047552           307200
32                  131072        Standard_E16d_v4        16               1047552           614400
32                  163840        Standard_E20d_v4        20               1047552           768000
32                  262144        Standard_E32d_v4        32               1047552           1228800
32                  393216        Standard_E48d_v4        48               1047552           1843200
32                  516096        Standard_E64d_v4        64               1047552           2457600
4                   16384         Standard_E2d_v5         2                1047552           76800
8                   32768         Standard_E4d_v5         4                1047552           153600
16                  65536         Standard_E8d_v5         8                1047552           307200
32                  131072        Standard_E16d_v5        16               1047552           614400
32                  163840        Standard_E20d_v5        20               1047552           768000
32                  262144        Standard_E32d_v5        32               1047552           1228800
32                  393216        Standard_E48d_v5        48               1047552           1843200
32                  524288        Standard_E64d_v5        64               1047552           2457600
32                  688128        Standard_E96d_v5        96               1047552           3686400
64                  688128        Standard_E104id_v5      104              1047552           3891200
4                   16384         Standard_E2s_v3         2                1047552           32768
8                   32768         Standard_E4-2s_v3       4                1047552           65536
8                   32768         Standard_E4s_v3         4                1047552           65536
16                  65536         Standard_E8-2s_v3       8                1047552           131072
16                  65536         Standard_E8-4s_v3       8                1047552           131072
16                  65536         Standard_E8s_v3         8                1047552           131072
32                  131072        Standard_E16-4s_v3      16               1047552           262144
32                  131072        Standard_E16-8s_v3      16               1047552           262144
32                  131072        Standard_E16s_v3        16               1047552           262144
32                  163840        Standard_E20s_v3        20               1047552           327680
32                  262144        Standard_E32-8s_v3      32               1047552           524288
32                  262144        Standard_E32-16s_v3     32               1047552           524288
32                  262144        Standard_E32s_v3        32               1047552           524288
32                  393216        Standard_E48s_v3        48               1047552           786432
32                  442368        Standard_E64-16s_v3     64               1047552           884736
32                  442368        Standard_E64-32s_v3     64               1047552           884736
32                  442368        Standard_E64s_v3        64               1047552           884736
4                   16384         Standard_E2s_v4         2                1047552           0
8                   32768         Standard_E4-2s_v4       4                1047552           0
8                   32768         Standard_E4s_v4         4                1047552           0
16                  65536         Standard_E8-2s_v4       8                1047552           0
16                  65536         Standard_E8-4s_v4       8                1047552           0
16                  65536         Standard_E8s_v4         8                1047552           0
32                  131072        Standard_E16-4s_v4      16               1047552           0
32                  131072        Standard_E16-8s_v4      16               1047552           0
32                  131072        Standard_E16s_v4        16               1047552           0
32                  163840        Standard_E20s_v4        20               1047552           0
32                  262144        Standard_E32-8s_v4      32               1047552           0
32                  262144        Standard_E32-16s_v4     32               1047552           0
32                  262144        Standard_E32s_v4        32               1047552           0
32                  393216        Standard_E48s_v4        48               1047552           0
32                  516096        Standard_E64-16s_v4     64               1047552           0
32                  516096        Standard_E64-32s_v4     64               1047552           0
32                  516096        Standard_E64s_v4        64               1047552           0
4                   16384         Standard_E2s_v5         2                1047552           0
8                   32768         Standard_E4-2s_v5       4                1047552           0
8                   32768         Standard_E4s_v5         4                1047552           0
16                  65536         Standard_E8-2s_v5       8                1047552           0
32                  65536         Standard_E8-4s_v5       8                1047552           0
16                  65536         Standard_E8s_v5         8                1047552           0
32                  131072        Standard_E16-4s_v5      16               1047552           0
32                  131072        Standard_E16-8s_v5      16               1047552           0
32                  131072        Standard_E16s_v5        16               1047552           0
32                  163840        Standard_E20s_v5        20               1047552           0
32                  262144        Standard_E32-8s_v5      32               1047552           0
32                  262144        Standard_E32-16s_v5     32               1047552           0
32                  262144        Standard_E32s_v5        32               1047552           0
32                  393216        Standard_E48s_v5        48               1047552           0
32                  524288        Standard_E64-16s_v5     64               1047552           0
32                  524288        Standard_E64-32s_v5     64               1047552           0
32                  524288        Standard_E64s_v5        64               1047552           0
32                  688128        Standard_E96-24s_v5     96               1047552           0
32                  688128        Standard_E96-48s_v5     96               1047552           0
32                  688128        Standard_E96s_v5        96               1047552           0
64                  688128        Standard_E104is_v5      104              1047552           0
4                   16384         Standard_E2_v3          2                1047552           51200
8                   32768         Standard_E4_v3          4                1047552           102400
16                  65536         Standard_E8_v3          8                1047552           204800
32                  131072        Standard_E16_v3         16               1047552           409600
32                  163840        Standard_E20_v3         20               1047552           512000
32                  262144        Standard_E32_v3         32               1047552           819200
32                  393216        Standard_E48_v3         48               1047552           1228800
32                  442368        Standard_E64_v3         64               1047552           1638400
4                   16384         Standard_E2_v4          2                1047552           0
8                   32768         Standard_E4_v4          4                1047552           0
16                  65536         Standard_E8_v4          8                1047552           0
32                  131072        Standard_E16_v4         16               1047552           0
32                  163840        Standard_E20_v4         20               1047552           0
32                  262144        Standard_E32_v4         32               1047552           0
32                  393216        Standard_E48_v4         48               1047552           0
32                  516096        Standard_E64_v4         64               1047552           0
4                   16384         Standard_E2_v5          2                1047552           0
8                   32768         Standard_E4_v5          4                1047552           0
16                  65536         Standard_E8_v5          8                1047552           0
32                  131072        Standard_E16_v5         16               1047552           0
32                  163840        Standard_E20_v5         20               1047552           0
32                  262144        Standard_E32_v5         32               1047552           0
32                  393216        Standard_E48_v5         48               1047552           0
32                  524288        Standard_E64_v5         64               1047552           0
32                  688128        Standard_E96_v5         96               1047552           0
64                  688128        Standard_E104i_v5       104              1047552           0
4                   16384         Standard_E2bs_v5        2                1047552           0
8                   32768         Standard_E4bs_v5        4                1047552           0
16                  65536         Standard_E8bs_v5        8                1047552           0
32                  131072        Standard_E16bs_v5       16               1047552           0
32                  262144        Standard_E32bs_v5       32               1047552           0
32                  393216        Standard_E48bs_v5       48               1047552           0
32                  524288        Standard_E64bs_v5       64               1047552           0
4                   16384         Standard_E2bds_v5       2                1047552           76800
8                   32768         Standard_E4bds_v5       4                1047552           153600
16                  65536         Standard_E8bds_v5       8                1047552           307200
32                  131072        Standard_E16bds_v5      16               1047552           614400
32                  262144        Standard_E32bds_v5      32               1047552           1228800
32                  393216        Standard_E48bds_v5      48               1047552           1843200
32                  524288        Standard_E64bds_v5      64               1047552           2457600
2                   512           Standard_B1ls           1                1047552           4096
2                   2048          Standard_B1ms           1                1047552           4096
2                   1024          Standard_B1s            1                1047552           4096
4                   8192          Standard_B2ms           2                1047552           16384
4                   4096          Standard_B2s            2                1047552           8192
8                   16384         Standard_B4ms           4                1047552           32768
16                  32768         Standard_B8ms           8                1047552           65536
16                  49152         Standard_B12ms          12               1047552           98304
32                  65536         Standard_B16ms          16               1047552           131072
32                  81920         Standard_B20ms          20               1047552           163840
4                   2048          Standard_F1             1                1047552           16384
8                   4096          Standard_F2             2                1047552           32768
16                  8192          Standard_F4             4                1047552           65536
32                  16384         Standard_F8             8                1047552           131072
64                  32768         Standard_F16            16               1047552           262144
4                   2048          Standard_F1s            1                1047552           4096
8                   4096          Standard_F2s            2                1047552           8192
16                  8192          Standard_F4s            4                1047552           16384
32                  16384         Standard_F8s            8                1047552           32768
64                  32768         Standard_F16s           16               1047552           65536
2                   2048          Standard_A1_v2          1                1047552           10240
4                   16384         Standard_A2m_v2         2                1047552           20480
4                   4096          Standard_A2_v2          2                1047552           20480
8                   32768         Standard_A4m_v2         4                1047552           40960
8                   8192          Standard_A4_v2          4                1047552           40960
16                  65536         Standard_A8m_v2         8                1047552           81920
16                  16384         Standard_A8_v2          8                1047552           81920
8                   28672         Standard_G1             2                1047552           393216
16                  57344         Standard_G2             4                1047552           786432
32                  114688        Standard_G3             8                1047552           1572864
64                  229376        Standard_G4             16               1047552           3145728
64                  458752        Standard_G5             32               1047552           6291456
8                   28672         Standard_GS1            2                1047552           57344
16                  57344         Standard_GS2            4                1047552           114688
32                  114688        Standard_GS3            8                1047552           229376
64                  229376        Standard_GS4            16               1047552           458752
64                  229376        Standard_GS4-4          16               1047552           458752
64                  229376        Standard_GS4-8          16               1047552           458752
64                  458752        Standard_GS5            32               1047552           917504
64                  458752        Standard_GS5-8          32               1047552           917504
64                  458752        Standard_GS5-16         32               1047552           917504
16                  32768         Standard_L4s            4                1047552           694272
32                  65536         Standard_L8s            8                1047552           1421312
64                  131072        Standard_L16s           16               1047552           2874368
64                  262144        Standard_L32s           32               1047552           5765120
32                  442368        Standard_E64i_v3        64               1047552           1638400
32                  442368        Standard_E64is_v3       64               1047552           884736
64                  1024000       Standard_M64            64               1047552           8192000
64                  1792000       Standard_M64m           64               1047552           8192000
64                  2048000       Standard_M128           128              1047552           16384000
64                  3891200       Standard_M128m          128              1047552           16384000
8                   224000        Standard_M8-2ms         8                1047552           256000
8                   224000        Standard_M8-4ms         8                1047552           256000
8                   224000        Standard_M8ms           8                1047552           256000
16                  448000        Standard_M16-4ms        16               1047552           512000
16                  448000        Standard_M16-8ms        16               1047552           512000
16                  448000        Standard_M16ms          16               1047552           512000
32                  896000        Standard_M32-8ms        32               1047552           1024000
32                  896000        Standard_M32-16ms       32               1047552           1024000
32                  262144        Standard_M32ls          32               1047552           1024000
32                  896000        Standard_M32ms          32               1047552           1024000
32                  196608        Standard_M32ts          32               1047552           1024000
64                  1792000       Standard_M64-16ms       64               1047552           2048000
64                  1792000       Standard_M64-32ms       64               1047552           2048000
64                  524288        Standard_M64ls          64               1047552           2048000
64                  1792000       Standard_M64ms          64               1047552           2048000
64                  1024000       Standard_M64s           64               1047552           2048000
64                  3891200       Standard_M128-32ms      128              1047552           4096000
64                  3891200       Standard_M128-64ms      128              1047552           4096000
64                  3891200       Standard_M128ms         128              1047552           4096000
64                  2048000       Standard_M128s          128              1047552           4096000
32                  896000        Standard_M32ms_v2       32               1047552           0
64                  1835008       Standard_M64ms_v2       64               1047552           0
64                  1048576       Standard_M64s_v2        64               1047552           0
64                  3985408       Standard_M128ms_v2      128              1047552           0
64                  2097152       Standard_M128s_v2       128              1047552           0
64                  4194304       Standard_M192ims_v2     192              1047552           0
64                  2097152       Standard_M192is_v2      192              1047552           0
32                  896000        Standard_M32dms_v2      32               1047552           1048576
64                  1835008       Standard_M64dms_v2      64               1047552           2097152
64                  1048576       Standard_M64ds_v2       64               1047552           2097152
64                  3985408       Standard_M128dms_v2     128              1047552           4194304
64                  2097152       Standard_M128ds_v2      128              1047552           4194304
64                  4194304       Standard_M192idms_v2    192              1047552           4194304
64                  2097152       Standard_M192ids_v2     192              1047552           4194304
4                   4096          Standard_F2s_v2         2                1047552           16384
8                   8192          Standard_F4s_v2         4                1047552           32768
16                  16384         Standard_F8s_v2         8                1047552           65536
32                  32768         Standard_F16s_v2        16               1047552           131072
32                  65536         Standard_F32s_v2        32               1047552           262144
32                  98304         Standard_F48s_v2        48               1047552           393216
32                  131072        Standard_F64s_v2        64               1047552           524288
32                  147456        Standard_F72s_v2        72               1047552           589824
8                   14336         Standard_NV4as_v4       4                1047552           90112
16                  28672         Standard_NV8as_v4       8                1047552           180224
32                  57344         Standard_NV16as_v4      16               1047552           360448
32                  114688        Standard_NV32as_v4      32               1047552           720896
64                  516096        Standard_E80is_v4       80               1047552           0
64                  516096        Standard_E80ids_v4      80               1047552           4362240
1                   768           Standard_A0             1                1047552           20480
2                   1792          Standard_A1             1                1047552           71680
4                   3584          Standard_A2             2                1047552           138240
8                   7168          Standard_A3             4                1047552           291840
4                   14336         Standard_A5             2                1047552           138240
16                  14336         Standard_A4             8                1047552           619520
8                   28672         Standard_A6             4                1047552           291840
16                  57344         Standard_A7             8                1047552           619520
1                   768           Basic_A0                1                1047552           20480
2                   1792          Basic_A1                1                1047552           40960
4                   3584          Basic_A2                2                1047552           61440
8                   7168          Basic_A3                4                1047552           122880
16                  14336         Basic_A4                8                1047552           245760
8                   32768         Standard_DC8_v2         8                1047552           409600
1                   4096          Standard_DC1s_v2        1                1047552           51200
2                   8192          Standard_DC2s_v2        2                1047552           102400
4                   16384         Standard_DC4s_v2        4                1047552           204800
4                   3584          Standard_D1             1                1047552           51200
8                   7168          Standard_D2             2                1047552           102400
16                  14336         Standard_D3             4                1047552           204800
32                  28672         Standard_D4             8                1047552           409600
8                   14336         Standard_D11            2                1047552           102400
16                  28672         Standard_D12            4                1047552           204800
32                  57344         Standard_D13            8                1047552           409600
64                  114688        Standard_D14            16               1047552           819200
4                   3584          Standard_DS1            1                1047552           7168
8                   7168          Standard_DS2            2                1047552           14336
16                  14336         Standard_DS3            4                1047552           28672
32                  28672         Standard_DS4            8                1047552           57344
8                   14336         Standard_DS11           2                1047552           28672
16                  28672         Standard_DS12           4                1047552           57344
32                  57344         Standard_DS13           8                1047552           114688
64                  114688        Standard_DS14           16               1047552           229376
16                  65536         Standard_L8s_v2         8                1047552           81920
32                  131072        Standard_L16s_v2        16               1047552           163840
32                  262144        Standard_L32s_v2        32               1047552           327680
32                  393216        Standard_L48s_v2        48               1047552           491520
32                  524288        Standard_L64s_v2        64               1047552           655360
32                  655360        Standard_L80s_v2        80               1047552           819200
4                   8192          Standard_D2a_v4         2                1047552           51200
8                   16384         Standard_D4a_v4         4                1047552           102400
16                  32768         Standard_D8a_v4         8                1047552           204800
32                  65536         Standard_D16a_v4        16               1047552           409600
32                  131072        Standard_D32a_v4        32               1047552           819200
32                  196608        Standard_D48a_v4        48               1047552           1228800
32                  262144        Standard_D64a_v4        64               1047552           1638400
32                  393216        Standard_D96a_v4        96               1047552           2457600
4                   8192          Standard_D2as_v4        2                1047552           16384
8                   16384         Standard_D4as_v4        4                1047552           32768
16                  32768         Standard_D8as_v4        8                1047552           65536
32                  65536         Standard_D16as_v4       16               1047552           131072
32                  131072        Standard_D32as_v4       32               1047552           262144
32                  196608        Standard_D48as_v4       48               1047552           393216
32                  262144        Standard_D64as_v4       64               1047552           524288
32                  393216        Standard_D96as_v4       96               1047552           786432
4                   16384         Standard_E2a_v4         2                1047552           51200
8                   32768         Standard_E4a_v4         4                1047552           102400
16                  65536         Standard_E8a_v4         8                1047552           204800
32                  131072        Standard_E16a_v4        16               1047552           409600
32                  163840        Standard_E20a_v4        20               1047552           512000
32                  262144        Standard_E32a_v4        32               1047552           819200
32                  393216        Standard_E48a_v4        48               1047552           1228800
32                  524288        Standard_E64a_v4        64               1047552           1638400
32                  688128        Standard_E96a_v4        96               1047552           2457600
4                   16384         Standard_E2as_v4        2                1047552           32768
8                   32768         Standard_E4-2as_v4      4                1047552           65536
8                   32768         Standard_E4as_v4        4                1047552           65536
16                  65536         Standard_E8-2as_v4      8                1047552           131072
16                  65536         Standard_E8-4as_v4      8                1047552           131072
16                  65536         Standard_E8as_v4        8                1047552           131072
32                  131072        Standard_E16-4as_v4     16               1047552           262144
32                  131072        Standard_E16-8as_v4     16               1047552           262144
32                  131072        Standard_E16as_v4       16               1047552           262144
32                  163840        Standard_E20as_v4       20               1047552           327680
32                  262144        Standard_E32-8as_v4     32               1047552           524288
32                  262144        Standard_E32-16as_v4    32               1047552           524288
32                  262144        Standard_E32as_v4       32               1047552           524288
32                  393216        Standard_E48as_v4       48               1047552           786432
32                  524288        Standard_E64-16as_v4    64               1047552           884736
32                  524288        Standard_E64-32as_v4    64               1047552           884736
32                  524288        Standard_E64as_v4       64               1047552           884736
32                  688128        Standard_E96-24as_v4    96               1047552           1376256
32                  688128        Standard_E96-48as_v4    96               1047552           1376256
32                  688128        Standard_E96as_v4       96               1047552           1376256
32                  688128        Standard_E96ias_v4      96               1047552           1376256
32                  57344         Standard_H8             8                1047552           1024000
32                  57344         Standard_H8_Promo       8                1047552           1024000
64                  114688        Standard_H16            16               1047552           2048000
64                  114688        Standard_H16_Promo      16               1047552           2048000
32                  114688        Standard_H8m            8                1047552           1024000
32                  114688        Standard_H8m_Promo      8                1047552           1024000
64                  229376        Standard_H16m           16               1047552           2048000
64                  229376        Standard_H16m_Promo     16               1047552           2048000
64                  114688        Standard_H16r           16               1047552           2048000
64                  114688        Standard_H16r_Promo     16               1047552           2048000
64                  229376        Standard_H16mr          16               1047552           2048000
64                  229376        Standard_H16mr_Promo    16               1047552           2048000
12                  114688        Standard_NC6s_v3        6                1047552           344064
24                  229376        Standard_NC12s_v3       12               1047552           688128
32                  458752        Standard_NC24rs_v3      24               1047552           1376256
32                  458752        Standard_NC24s_v3       24               1047552           1376256
64                  5836800       Standard_M208ms_v2      208              1047552           4194304
64                  2918400       Standard_M208s_v2       208              1047552           4194304
64                  5836800       Standard_M416-208s_v2   416              1047552           8388608
64                  5836800       Standard_M416s_v2       416              1047552           8388608
64                  11673600      Standard_M416-208ms_v2  416              1047552           8388608
64                  11673600      Standard_M416ms_v2      416              1047552           8388608
32                  466944        Standard_HB120rs_v2     120              1047552           491520
12                  114688        Standard_NV6s_v2        6                1047552           344064
24                  229376        Standard_NV12s_v2       12               1047552           688128
32                  458752        Standard_NV24s_v2       24               1047552           1376256
12                  114688        Standard_NV12s_v3       12               1047552           344064
24                  229376        Standard_NV24s_v3       24               1047552           688128
32                  458752        Standard_NV48s_v3       48               1047552           1376256
4                   360448        Standard_HC44-16rs      44               1047552           716800
4                   360448        Standard_HC44-32rs      44               1047552           716800
4                   360448        Standard_HC44rs         44               1047552           716800
24                  57344         Standard_NV6            6                1047552           389120
48                  114688        Standard_NV12           12               1047552           696320
64                  229376        Standard_NV24           24               1047552           1474560
24                  57344         Standard_NV6_Promo      6                1047552           389120
48                  114688        Standard_NV12_Promo     12               1047552           696320
64                  229376        Standard_NV24_Promo     24               1047552           1474560
8                   28672         Standard_NC4as_T4_v3    4                1047552           180224
16                  57344         Standard_NC8as_T4_v3    8                1047552           360448
32                  112640        Standard_NC16as_T4_v3   16               1047552           360448
32                  450560        Standard_NC64as_T4_v3   64               1047552           2883584
24                  57344         Standard_NC6            6                1047552           389120
48                  114688        Standard_NC12           12               1047552           696320
64                  229376        Standard_NC24           24               1047552           1474560
64                  229376        Standard_NC24r          24               1047552           1474560
24                  57344         Standard_NC6_Promo      6                1047552           389120
48                  114688        Standard_NC12_Promo     12               1047552           696320
64                  229376        Standard_NC24_Promo     24               1047552           1474560
64                  229376        Standard_NC24r_Promo    24               1047552           1474560
4                   8192          Standard_DC1s_v3        1                1047552           0
8                   16384         Standard_DC2s_v3        2                1047552           0
16                  32768         Standard_DC4s_v3        4                1047552           0
32                  65536         Standard_DC8s_v3        8                1047552           0
32                  131072        Standard_DC16s_v3       16               1047552           0
32                  196608        Standard_DC24s_v3       24               1047552           0
32                  262144        Standard_DC32s_v3       32               1047552           0
32                  393216        Standard_DC48s_v3       48               1047552           0
4                   8192          Standard_DC1ds_v3       1                1047552           76800
8                   16384         Standard_DC2ds_v3       2                1047552           153600
16                  32768         Standard_DC4ds_v3       4                1047552           307200
32                  65536         Standard_DC8ds_v3       8                1047552           614400
32                  131072        Standard_DC16ds_v3      16               1047552           1228800
32                  196608        Standard_DC24ds_v3      24               1047552           1843200
32                  262144        Standard_DC32ds_v3      32               1047552           2457600
32                  393216        Standard_DC48ds_v3      48               1047552           2457600
Creating a Vm with size 
damilola@Azure:~$ az vm create \
>     --resource-group myResourceGroupVM \
>     --name myVM3 \
>     --image UbuntuLTS \
> --size Standard_B1s \
>  --generate-ssh-keys

{
  "fqdns": "",
  "id": "/subscriptions/4806a9a7-e202-49ac-856e-ec8fa95a2c39/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM3",
  "location": "australiaeast",
  "macAddress": "00-22-48-94-5E-71",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.6",
  "publicIpAddress": "20.213.94.137",
  "resourceGroup": "myResourceGroupVM",
  "zones": ""
}
You can view the current of size of a VM with az vm show:
damilola@Azure:~$ az vm show --resource-group myResourceGroupVM --name myVM --query hardwareProfile.vmSize
"Standard_DS1_v2"




6. VM power states
To retrieve the state of a particular VM, use the az vm get-instance-view command. Be sure to specify a valid name for a virtual machine and resource group.

damilola@Azure:~$ az vm get-instance-view \
>     --name myVM \
>     --resource-group myResourceGroupVM \
>     --query instanceView.statuses[1] --output table
Code                Level    DisplayStatus
------------------  -------  ---------------
PowerState/running  Info     VM running



7. Management tasks
This command returns the private and public IP addresses of a virtual machine.
damilola@Azure:~$ az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table
VirtualMachine    PublicIPAddresses    PrivateIPAddresses
----------------  -------------------  --------------------
myVM              20.211.161.18        10.0.0.4

Stop virtual machine
damilola@Azure:~$ az vm stop --resource-group myResourceGroupVM --name myVM
About to power off the specified VM...
It will continue to be billed. To deallocate a VM, run: az vm deallocate.

Start virtual machine
damilola@Azure:~$ az vm start --resource-group myResourceGroupVM --name myVM


Deleting VM resources

Notes:
Quickstart: Create a Linux VM

https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-manage-vm
Quickstart for Bash in Azure Cloud Shell

https://docs.microsoft.com/en-us/azure/cloud-shell/quickstart