Create a VPC with the following property using the CLI

`aws ec2 create-vpc --cidr-block 10.0.0.0/16 --query Vpc.VpcId --output text`



`aws ec2 create-subnet --vpc-id vpc-00186153e673dfe8d  --cidr-block 10.0.1.0/24`
`aws ec2 create-subnet --vpc-id vpc-00186153e673dfe8d --cidr-block 10.0.0.0/24`
`aws ec2 create-internet-gateway --query InternetGateway.InternetGatewayId --output text`
output: `igw-028cdcb635850852d`
`aws ec2 attach-internet-gateway --vpc-id vpc-00186153e673dfe8d --internet-gateway-id igw-028cdcb635850852d`

Create a custom route table for your VPC using the following create-route-table command:
`aws ec2 create-route-table --vpc-id vpc-00186153e673dfe8d --query RouteTable.RouteTableId --output text`
output:`rtb-0ae284696615db6d0`

Create a route in the route table that points all traffic (0.0.0.0/0) to the internet gateway using the following create-route command:`aws ec2 create-route --route-table-id rtb-0ae284696615db6d0 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-028cdcb635850852d`
Returned output :'True'
`aws ec2 describe-subnets --filters "Name=vpc-id,Values=vpc-00186153e673dfe8d" --query "Subnets[*].{ID:SubnetId,CIDR:CidrBlock}"`


`aws ec2 associate-route-table  --subnet-id subnet-03bd96f43332c76a6 --route-table-id rtb-0ae284696615db6d0`
