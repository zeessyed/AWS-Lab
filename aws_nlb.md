# Create Network Load Balancer # 
Lab is for when an Application running on Custom TCP port 
# Topology is look like 
> We have a VPC-A where copuple of instances running on Target Group (TG) and we have a Network Load Balancer 
> Then We will assign Elastic IP address to the subnet in two different AZ 
> That means NLB will have  static public address 
> In other hand we have another VPC called VAPC-B where client running an Application where we can use NACL 
  Why we use NACL? 
  Ans: Most company does not allow port 8181, they will restrict all outbound port from the VPC or from the NW
       Please refer the below box for inbound and outbound rules 

Rule	Portrange	Destination	      Allow / Deny
97	    8181	      elastic-ip-1	           ALLOW
98	    8181	      elastic-ip-2	           ALLOW
99	    8181	       0.0.0.0/0	           DENY
100	    ALL	           0.0.0.0/0	           ALLOW
*	    ALL	           0.0.0.0/0	           DENY

According to Allow port client should access the NLB in two subnet. 

# Now let's create the app 

Step 1:
# Create Template
> Go to EC2 and under Launch Template , Click at "Create Launch Template"
> Put Template Name ( For my case : CustomSas)
> Slowly down and select AMI (Amazon Linux 2023 )
> Instance type t2 micro 
> Key pair (optional) , but I prefered select key pair 
> Network leave as it it only select Security Group as Esisting for my case (webaccess)
> Under advance section just copy and paste nlb-user-data.sh ( Please copy and paste the script)

#!/bin/bash
# Update the system
yum update -y
# Install Python 3
yum install -y python3
# Create a simple Python web server script
cat > /home/ec2-user/webserver.py << EOF
import http.server
import socketserver

PORT = 8181
Handler = http.server.SimpleHTTPRequestHandler

class CustomHandler(Handler):
    def do_GET(self):
        self.send_response(200)
        self.send_header('Content-type', 'text/html')
        self.end_headers()
        self.wfile.write(bytes("Welcome to My Custom SaaS App", "utf8"))

httpd = socketserver.TCPServer(("", PORT), CustomHandler)
print("serving at port", PORT)
httpd.serve_forever()
EOF
# Run the web server in the background
nohup python3 /home/ec2-user/webserver.py > /dev/null 2>&1 &

The above code just create a simple python flusk code having port number 8181 

 > Copy the above code and paste under user data section then click "Create Launch Template"

 Step 2:
 # Create Auto Scaling Group
> Now come to ASG , Click at "Create Auto Scaling Group" 
> Put the Name as ASG 
> Select Launch Template as CustomSas (which I created)
> Click on Next -> Leave at Defalut VPC 
> Select two AZ as us-east-1a & 1b
> Click on Next -> No Load Balancer this point of time -> Click on Next 
> Desire Capacity 2 , Scaling limit minimum is 2 and maximum is 2 
> Click on "Create Launch"

Step 3:
> Go to Security group from Netwwork & Security 
> Select Webaccess --> Under Inbound rule make sure port 8181 from any source 
> Now click on "Elastic IP" -> Click on Allocate --> Leave the default settings --> Click on Allocate 
> Do the same again 
> We can see two Elastic IP 

Step 4:
# Create a Target Group 
> Under Target Group --> Click on "Create Target Group" --> Target type would be instances 
> Put Target Group Name (My case : TG-NLB) 
> Porotocol: Port would be TCP --> Put 8181 
> Change Health Check as TCP 
> Click on Next --> Now select two instaces--> Click "Include as pending below"
> Click on "Create teh Targer Group"

Step 5:
# Create a Load Balancer 
>  Go to Load Balancer 
> Click on "Create Load Balancer" -> Choose NLB --> Put Load Balancer Name (my case:nlb ) 
> Internet facing --> IPV4 ---> Default VPC --> Subnet us-east-1a & 1b 
> Use Elastic IP for both Subnet  (Make sure you will choose two different Elastic IPS)
> Select Security Group (Webaccess)
> Listener Port should be : 8181 & Protocol : TCP 
> Select the Target group from drop down 
> Click on "Create Load Balancer" 

Step 6:
# Create another instance in Different AZ but in defalut VPC fro that you just change your region 
> Create an instance as you did 
> After create the instance go to VPC console 
> Under Security --> Network ACL --> Make sure it is in different Region 
> Select the Network ACL ---> Under Outbound Rules --> Click on "Edit outbound rules" --> Now basically I will create the outbound rules with port 97 , 98 & 99 
> For port 98 port range 8181 --> Then go to previous region --> Back in the Load balancer 
> Under Network Mapping --> We can see the Elastic ip or during assign Elastic IP you can copy those IP 
> Select the First one Elastic IP and paste in under Destination where you set port 98 
> Put CIDR block range is /32 
> For port 98 same only copy second one Elastic IP and CIDR block range /32 
> For port 99 make sure we deny for any destination 
> Click on save rules 

===================================
# Now let's see we can connect our applications or not 
> Now the DNS name from load balancer 
> Connect the instance (Which you create in different Region)
> Now run the curl command with the DNS name as 
  curl TG-NLB-7e0c0d39b938e694.elb.us-east-1.amazonaws.com:8181
  You can see Welcome to My Custom SaaS App 
> Now come again in different region , under Security --> Network ACL --> Just delete or deny the two outbound rules for port 97 & 98 and run the same curl command . then you can not see the o/p as 

Welcome to My Custom SaaS App 
That means we broke the access 
===============================
Hope you enjoy your lab 
Drain all the EC2 instances 
Delete Load balancer 
Deleet Auto Scaling 
Release Elastic IP address 



    
