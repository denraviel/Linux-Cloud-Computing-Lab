Launch an instance 
created security group:`aws ec2 create-security-group --group-name serverSG --description "security group instance" --vpc-id vpc-00186153e673dfe8d`
Connected  port 22 :`aws ec2 authorize-security-group-ingress --group-id sg-0cb518ec03a93cacb --protocol tcp --port 22 --cidr 0.0.0.0/0`
Connected port 80 : `aws ec2 authorize-security-group-ingress --group-id sg-0cb518ec03a93cacb --protocol tcp --port 80 --cidr 0.0.0.0/0`
 
Created instace with following properties; key pair':`aws ec2 run-instances --image-id ami-00af37d1144686454 --count 1 --instance-type t2.micro --key-name ProdCafeServer --security-group-ids sg-0d439eaf2b2ea9cbc --subnet-id subnet-0a93f710e3a61bbbc`

 Associate an elastic ip with this instance, you will need it in later lab
 `aws ec2 allocate-address`

 Associate an Elastic IP address with an instance or network interface
 `aws ec2 associate-address --instance-id i-0b94d6126c35ae16b --public-ip 44.225.208.140`
 