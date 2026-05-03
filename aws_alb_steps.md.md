# Create an Application Load Balancer 
Step 1( Create Traget Group under LOad Balancer):
> Please complete Auto Scaling and Auto Scaling Group then do as below 
# Steps for Create ALB #
We're gonna put an ALB in front of our auto scaling
group and we're gonna direct traffic to the application load balancer.
> After comple ASG creation 
> Come to Load Balancer 
> Click on Target Group 
> Create Traget Group on Instance as ALB 
> Click Create the Target Group 

Step 2 (Create Load Balancer):
> Click on Load Balancer 
> Click "Create Load Balancer"
> Select "Application LB"
> Put Load balancer name -- ALB1 
> Availability Zones and subnets (us-east-1a & 1b because my instance are deployed at this two AZ)
> Security groups ( Choose Webaccess) , select which you created during deploy your instance 
> Under Listener and Routing section select Target Group name from the drop down (my case it is TG1)

Step 3 (Assign Target Group/ Register the Traget Group):
> Click on Traget Group 
> After refresh you can't see the TG1 
=====================================
> Click on Auto Scaling Group 
> Under Integrations tab  you can see Load Balancer 
> Click on Edit 
> Select Application Network or Gateway Load Balancer 
> From drop down select Target Group (my case TG1)
> Click update 
> Now back to Target Group , you can see the Registered targets ( means instances are registered at Target Group)
=========================================
So we have the traffic being spread equally between those two AZs ( Different Data Center)
So that's it for this lesson,the load balance is up and running auto scaling is working.
in the next lesson. What we're gonna do is create a scaling policy.