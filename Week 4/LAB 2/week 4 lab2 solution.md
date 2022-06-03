Launch an instance 
created security group:`aws ec2 create-security-group --group-name serverSG --description "security group instance" --vpc-id vpc-087059980fdfe1503`
Connected  port 22 :`aws ec2 authorize-security-group-ingress --group-id sg-08aa4aef574ce556e --protocol tcp --port 22 --cidr 0.0.0.0/0`
Connected port 80 : `aws ec2 authorize-security-group-ingress --group-id sg-08aa4aef574ce556e --protocol tcp --port 80 --cidr 0.0.0.0/0`
 
Created instace with following properties; key pair':`aws ec2 run-instances --image-id ami-0022f774911c1d690 --count 1 --instance-type t2.micro --key-name ProdCafeServer --security-group-ids sg-08aa4aef574ce556e --subnet-id subnet-01f9e1bf6ef59a104`