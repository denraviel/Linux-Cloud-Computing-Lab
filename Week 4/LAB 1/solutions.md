Create a VPC with the following property using the CLI

`aws ec2 create-vpc --cidr-block 10.0.0.0/16 --query Vpc.VpcId --output text`



`aws ec2 create-subnet --vpc-id vpc-03f7d33103205b3b6  --cidr-block 10.0.1.0/24`
`aws ec2 create-subnet --vpc-id vpc-03f7d33103205b3b6 --cidr-block 10.0.0.0/24`
`aws ec2 create-internet-gateway --query InternetGateway.InternetGatewayId --output text`
output: `igw-02bd007f880fc4a09`
`aws ec2 attach-internet-gateway --vpc-id vpc-03f7d33103205b3b6 --internet-gateway-id igw-02bd007f880fc4a09`

Create a custom route table for your VPC using the following create-route-table command:
`aws ec2 create-route-table --vpc-id vpc-03f7d33103205b3b6 --query RouteTable.RouteTableId --output text`
output:`rtb-0a828663e018af7a8`

Create a route in the route table that points all traffic (0.0.0.0/0) to the internet gateway using the following create-route command:`aws ec2 create-route --route-table-id rtb-0a828663e018af7a8 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-02bd007f880fc4a09`
Returned output :'True'