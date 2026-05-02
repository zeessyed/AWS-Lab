## EC2 AUTO SCALING Group Steps are:

Step 1:
> Click Create a Launch Template (Under Instance Section)
> Put Launch template name and description (my case: myweb_serevr)
> Select Amazon Machine Image : Amazon Linux 
> Select Instance type : t2 micro 
> Key-par (Optional) here I used my key pair 
> Security groups : webaccess (You can select your existing SG)
> Click Advanced details 
> Under User data - optional : simply paste the script 
> Click on "Create launch template"

Ste 2:
> From left hand side click "Auto scaling Groups" 
> Click on Create Auto Scaling group
> Put Auto Scaling group name ( ASG-1) [You can choose any name]
> Under Launch template select myweb_server ( The Template which you created)
> Click on Next 
> Select Default VPC
> Select two AZ (us-east-1a & us-east-1b)
> Click on Next > Again click Next 
> Set Desired capacity 2 (means number of instance)
> Min desired capacity & Max desired capacity (2)
> Click on "Next" 3 times 
> Click on "Create Auto Scaling group"

Step 3:
> Now go to EC2 and click instances , we can see two instances are running 
> Make sure port 80 is open 

Step 4:
> How do I check auto scaling policy properly configured or not 
> Click on any instance and just terminate 
# Once you terminate the any instance autoscling group will notice something is wrong and instance is no longer 
> Just wait some time and go to Autoscaling Group , keep on eye at Activity 
> You can see Autoscaling Group create couple of new instances at the Activity log 
> Come back under EC2 you can see a new insatnce 

