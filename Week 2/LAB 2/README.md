# Lab 2: Manage Linux VMs with the AWS CLI


1. Create virtual machine
    I created a key pair 'my key pair' with the command: `aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem`

    Then I created an Instance with an Amazon Linux 2 Kernel 5.10 AMI 2.0.20220426.0 x86_64 HVM gp2 image which is available for free tier and an istance type t2.micro: `aws ec2 run-instances --image-id ami-0022f774911c1d690 --instance-type t2.micro --key-name MyKeyPair`
2. Connect to VM
3. Understand VM images
4. Understand VM sizes
5. VM power states
6. Management tasks



Notes:
Quickstart: Create a Linux VM

https://aws.amazon.com/getting-started/launch-a-virtual-machine-B-0/
Using a custom Amazon machine image (AMI)

https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.customenv.html
Quickstart: Restart VM via CLI

https://docs.aws.amazon.com/cli/latest/reference/ec2/reboot-instances.html
Quickstart for AWS CloudShell

https://docs.aws.amazon.com/cloudshell/latest/userguide/working-with-cloudshell.html