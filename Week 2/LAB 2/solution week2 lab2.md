Created a security group :`aws ec2 create-security-group --group-name raveurban --description 'schull practice SG' --vpc-id vpc-095d30e35e066f00b`
Create virtual machine
Created an instance :`aws ec2 run-instances --image-id ami-0022f774911c1d690 --instance-type t2.micro --subnet-id subnet-0a1f227671882cf53 --count 1 --security-group-ids sg-07dbb715c329258d5 --key-name week2lab`

Understand VM images
    I Launched an instance and select a Community AMI configure the instance
    I connected the instance with SSH :`ssh -i week2lab.pem ec2-user@44.201.246.238`
VM power states
    Stop instance `aws ec2 stop-instances --instance-ids i-0d1affa6ff3c09641`
    Terminate instance `aws ec2 terminate-instances --instance-ids i-0d1affa6ff3c09641`

