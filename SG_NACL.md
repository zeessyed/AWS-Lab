# Security Group & NACL 
Here I will show how to use security grouo and NACL (Network Access Control Lists)    
# What is Security Group & NACL ?
Security Groups and Network Access Control Lists (NACLs) are the two primary virtual firewall layers used in AWS to secure your resources. While Security Groups act at the instance level, NACLs provide a broader layer of security at the subnet     
# Key DifferencesScope: 
** Scope: Security Groups act on instances, while NACLs act on subnets.
** Statefulness: Security Groups are stateful (auto-allow return traffic), whereas NACLs are stateless (require explicit return rules).
** Rules: Security Groups only support "allow" rules; NACLs support both "allow" and "deny".Evaluation: Security Groups evaluate all rules; NACLs evaluate rules in numerical order.
** Defaults: Security Groups deny all inbound by default; NACLs allow all traffic by default.
# How They Work TogetherTraffic travels through the NACL (subnet level) first, followed by the Security Group (instance level). Both must permit traffic for it to reach the resource
=====================================================================
# Steps:
Open AWS Console --> Go to Security Grouo --> Create Security Grouo as SG1 --> Same description --> Just add inbound rules add ssh rules with port 22
(VPC should be defalut VPC)
Now remove the out bound rules and click "Create Security group" 

Now launch an instance under defalut VPC --> Just under select security grouo as SG1--> Click on "Launch Instance" 
Once instanvce will be ready please connect the instance 
This instance allow the inbound traffic 
Just try to ping google.com you can see it's not pininging 
Why it is not working? : Because traffic is not allowing outbound.

If we want to ping this one how do I ping?

# Steps:
Go to security group --> Select SG1 --> Go to out bound tab --> Click on Edit --> Click on "Add Rule" --> Select "ICMP-IPV4" --> Destination "Any IP" --> Save the rules

Now come back to the insatnce and try to ping that.
We can see it's piniging. 

Now if we do 
curl http://google.com 
We can it is not allowing because I'm not allowing http outbound 

To do that we can set out bound rule under SG1 
Just edit the out bout rule and set Type is HTTP and Destination Anywhere --> Save the rule 
Now come back and le's try , we can see the response. 
Note: If it's say allow default rule then under out bound set Type as all and Destination "Any IPV4" 

That means we can initiate all out bound connection 

However, this particular server is now only allowing inbound connections on port 22.

To do this just go to the instance and copy user-data-simple-website.sh 
Save it from root , make it executable and execute it . 

After that go to cd /var/www/html 

If we go to the insatnce and copy the public ip and open it through a browser then we see it is hang 
To do that come back to Security Group --> Click on SG1 --> Click on Edit Inbound Rules --> Add Rule --> Select HTTP--> From Any source --> Save it 

Now go to the browser and refresh , you can see it's work 

=======================================================================
Now craete a seperate security group with name SG2 (Under defalut VPC) --> Don't allow any inbound rule 
Just save it 
Then launch an insatnce under SG2 in defalut VPC 

Now what I will do 
Copy the private IP from second insatnce and try to ping that one from instance one console 
I belive it never pinging 
I don't really want everyone on the internet to ping my instance or to be able to 

To do that 
Go to SG2 --> Click on Add an inbound rule --> Type "ICMP-IPV4" --> Source (Select Security group SG1)

Now I can ping that private IP which is belong to instance2 .

# *** This is called Security Group Chaining *** # 

-- Now go to VPC -- > Under "Security" click "Network ACLs" -- > We can 6 Subnet is there -- > Click there (We have defalut VPC) --
--> If we click there we can see few things under "Network ACL ID" --> Under Allow/Deny we can see Allow & Deny --
--> Click "Edit inbound rules" --> Let's add rule 101 --> All Trafic --> Select Deny and Save it 
Check what happen 

If we can refresh the web page it work , if we are trying to connect instance it will connect 

Now just change the rule and change the number from 101 to 99 then we can see the oder flips 

If we refresh the web page it's gone and insatnce con't work. 
That means restriced access complete.

# After complete the task you can terminate instances. 






