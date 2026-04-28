### Create a Private Subnet and deploy Bastion Host in Public  Subnet #####
Why we are doing this ?

Because we can connect the Private Subnet through the Public Subnet. 

Step 1:
> Go to VPC and find the defalut VPC , copy CI/DR 
> Check the CI/DR block range 
> Then go to subnet order by defalut VPC under Public Subnet where you can see 
  CI/DR block range , arrange them by order
> Now create a subnet with VPC ID ( This should be defalut VPC) 
> Put IPv4 subnet CIDR block (172.31.80.0/20) 
   Why I choose 172.31.64.0 , because when we order the CI/DR block we can see last host id
   increase with 16 , if that is the case then previous CIDR block (172.31.96.0/20)
> Click create subnet

Step 2:
Now we need a seperate route table because private subnet always need a seperate route table. 

> Create a Route Table from "Route table"
> Put Name - optional (Default-VPC-private)
> Select default VPC 
> Click Create 
> Under subnet associations (Click "Edit subnet associations")
> Check pv-subnet-1a and click "Save associations"

Step 3:

## Now Launch couple of instances (First one will be private instance)
> For launching Private instance just select subnet name is private subnet which you created in my case (pv-subnet-1a)
> Security group will be same
> Launch the instance 

## Now create a Bastion Host (It's nothing just Launch a Public instance)
> Create a public instance as you did only select the subnet name us-east-1a 
> Rest of are same 

## Now connect public instance first from the public instance you will connect private instance 

This is basically work as a jump server (Those people are working in real time project they can understand about jump server)
Those are student please remember 

A Linux jump server (or bastion host) is a heavily secured, hardened intermediary server used to securely access private, internal network resources from a public network. 

Once you connect public instance then open your key pair .pem where you save it. Open with a text editor and copy the hole content of the key pair 

Now create that key pair with same name at uour /home/ec2-user path 
Put chmod 600 on the key pair file 

now do ssh -i <specify_the_key_pair> ec2-user@put_private_ip_address 
You can connect 

> Can I ping any other server in this jump server (idealy no)

## How do I ping other host at private subnet (private instance)
To do this we need VPC router where we configure to allow inbound and out bound rules

## This is called NAT Gateway 
NAT = The full form of NAT Gateway is Network Address Translation Gateway. It is a managed networking service used in cloud environments (like AWS or Azure) to allow instances in a private subnet to connect to the internet while preventing the internet from initiating connections with those

Step 4:
> Under VPC click NAT Gateway 
> Create NAT gateway
> Put Name - optional (NAT-GWP)
> Assign under public subnet , I select us-east-1a
> Allocate Elastic IP 
> Click Create NAT Gateway 

It will take some time to create 

Mean while we need to modify private route table 

> Under Route 
> Select Private Route 
> Click Edit 
> Click "Add route"
> Choose Destination 0.0.0./0 
> Select Target NAT Gateway 
> Click Save changes 

Now we can ping any host from BASTION host/Jump Server 

Note: NAT GW always be in public subnet 

## Important 

Please delete NAT GW , Release Elastic IP (can't do until NAT GW delete )
Terminate both instances 




  
