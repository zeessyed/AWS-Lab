# Create a Scaling Policy # 
In this lesson we're gonna create a scaling policy,so that we can set some dynamic scaling
on our auto scaling group and then generate some load and cause it to scale.

## After complete AWS ALB lab please do the following steps for this lab

Step 1:
> Go to Auto Scaling Group and click on Auto Scaling Grouo Name (ASG-1) 
> We can see Desired capacity 2 
> Our first task will be change the value of Desired capacity from 2 to 4 
> To do the above please click on and update Max desired capacity 4 
> Click on Update 
> As we increase Max desired capacity just like we should increase AZ 
> Click on Network ( At Auto Scaling Group) 
> Add more AZ us-east-1c & us-east-1d 
> Click on update 

Step 2:
We do the same thing for Load Balancer 
> Under Load Balancer 
> Click on ALB1 
> Click on Network Mapping 
> Click on Edit subnets under Availability Zones and subnets
> Make sure we also distribute the connection between us-east-1c  & us-east-1d 
> Click save

Step 3:
Now we should configure few thing at Auto Scaling 
> Click on Auto Scaling groups
> Click on ASG-1
> Click Automatic scaling tab 
> Click on Create dynamic scaling policy
> Policy type (leave at it is )
> Metric type would be Application Load Balancer request count per target
> Click Target group and select TG1 
> Leave the Target value 50 (That means one 50 connection per target , if we exceed that then it start scaling)
> Click create 

Step 4:
# Once Scaling policy set go to cloud watch 
> We can see some alarms created for us 
Alarm high is going to be triggered when the request count per target is greater
than 50 for free data points in three minutes.That means it's gonna scale out,but we've gotta wait, we've gotta generate load and then we've gotta wait for a few minutes
so that those data points come in.
Then after a while,as the request count per target gets lower, under 45 for 15 data points within 15 minutes,
then it will scale back in again.
> Now we are ready to scale 

Step 5:
> Come back to a Load balancer 
> Copy the DNS name my case (ALB1-1498860901.us-east-1.elb.amazonaws.com)
> # Command to generate load on the ALB

***replace with your alb dns name***
```for i in {1..200}; do curl http://your-alb-address.com & done; wait```

Just replace  the your-alb-address.com as  ALB1-1498860901.us-east-1.elb.amazonaws.com

# for i in {1..800}; do curl http://ALB1-1498860901.us-east-1.elb.amazonaws.com & done; wait

What it does. This is for loop which create 200 connections using load balancer by curl command 

> Open a cloud shell 
> Then paset the below connand 
for i in {1..800}; do curl http://ALB1-1498860901.us-east-1.elb.amazonaws.com & done; wait
> Press enter 
> We can see lots of connections are being made by load balancer 
> Once you can see okay at Cloud Watch alarm state 
> Back on Load Balancer page under Monitoring Tab 
> Again run the command few more times then you can see Alarm state In Alarm 
> Now come to Auto Scaling Group 
> Under Activity , we see the changes , couple of different entries , new instance based on target tracking alarm 
> Under insatnces you can see 4 instances are running 
> We can also see the Target Group 
> Go to Target 
> You can see 4 regirsterd TG 
> Now go to Load Balancer 
> Copy the DNS name 
> Put this in a browser window and hit enter 

## After complete the lab please drain the few resources 
> Auto Scaling Group 
> Delet Load Balancer 



