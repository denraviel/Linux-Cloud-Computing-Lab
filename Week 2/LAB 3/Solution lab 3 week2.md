1. Create a Linux VM
Created a Linux Vm on AWS:`aws ec2 run-instances --image-id ami-06eecef118bbf9259 --instance-type t2.micro --subnet-id subnet-02291a5ec55ccb635 --count 1 --security-group-ids sg-0fbdae7cca1255b9e --key-name tab2`

Having authorized protocol for port 22 in security group 'sg-0fbdae7cca1255b9e', I entered the directory i stored my key pair 'tab2.pem'in downloads using `cd downloads`Then i connected to the vm using command:`ssh -i tab2.pem ec2-user@54.197.84.157`

Install the Apache Web Server
I installed apache web server using the command:`sudo yum install httpd -y`

Start the service status via command line
I started the web server using :`sudo systemctl start httpd`

Enabled the web server :`sudo systemctl enable httpd`

Investigate the service status via command line    
I checked the status of the web server `sudo systemctl status httpd`, the output shows it is active 

Stop the service
stopped the service using command:`sudo service httpd stop`

checked the status again after stopping service the output shows that Apache server  stopped

