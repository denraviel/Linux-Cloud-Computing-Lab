I launched aws cloud shell on console 

I created a key pair'tab2' and saved it as a pem file :`aws ec2 create-key-pair --key-name morekeys --query 'keymaterial' --output text >tab2.pem`

Created a security group on the cli :`aws ec2 create-security-group --group-name raveurban --description 'schull practice SG' --vpc-id vpc-095d30e35e066f00b`

Created an Instance with command:`aws ec2 run-instances --image-id ami-06eecef118bbf9259 --instance-type t2.micro --subnet-id subnet-02291a5ec55ccb635 --count 1 --security-group-ids sg-0fbdae7cca1255b9e --key-name tab2`

To add a rule that allows inbound SSH traffic:`aws ec2 authorize-security-group-ingress --group-id sg-0fbdae7cca1255b9e --protocol tcp --port 22 --cidr 0.0.0.0/0`

To open port 80 that allows web traffic :`aws ec2 authorize-security-group-ingress --group-name raveurban --protocol tcp --port 80 --cidr 0.0.0.0/0`

Opened windows power shellLocated the directory where i saved my keypair 'tab2.pem', then i used ssh client to connect to instance:`ssh -i tab2.pem ec2-user@52.201.221.232`

Used `sudo yum update` to update packages on my system and download dependencies

I installed apache web server with `sudo yum install httpd -y`command 

