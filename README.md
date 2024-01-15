# Creating-A-Highly-Available-Environment-Lab

<h2>Description</h2>

This school project revolves around creating a Highly Available AWS environment using Workbench - Vocareum. 

1. Click **Start Lab** and wait until status says **Ready**. In the top right click the **AWS** button which will open the AWS management console.

<p align="center">
<br/>
<img src="https://i.imgur.com/ZDpDGoq.png"/>

<h2>Task 1: Inspecting your VPC</h2>

1. In the **services** menu in the top left click on **VPC**. In the left navigation pane click on **Filter by VPC**, choose **Select a VPC** box and select **Lab VPC**.
2. You now have access to **Subnet, Route Tables, Internet Gateways, Network ACLs, Security Groups, Inbound/Outbound rules** and much more.

<p align="center">
<br/>
<img src="https://i.imgur.com/HZ2pg33.png"/>

<h2>Task 2: Creating an Application Load Balancer</h2>

1. On the Services menu, choose EC2.
2. In the left navigation pane, choose Load Balancers.
3. Choose Create load balancer.
4. Application Load Balancer, choose Create.
5. Load balancer name, enter: Inventory-LB.
6. Scroll down to the Network mapping section, then for VPC, select Lab VPC.
7. Under Mappings, choose the first Availability Zone, then choose the Public Subnet that displays.
8. Choose the second Availability Zone, then choose the Public Subnet that displays.
9. In the Security groups section, select the Create a new security group hyperlink. This opens a new browser tab. Configure the new security group settings:
    - Security group name: Inventory-LB
    - Description: Enable web access to load balancer
    - VPC: Remove the default VPC by choosing the X to the right of it. Then select Lab VPC.
10. Under Inbound rules, choose Add rule and configure as described:
    - Type: HTTP
    - Source: Anywhere-IPv4
11. Still under Inbound rules, choose Add rule again and configure:
    - Type: HTTPS
    - Source: Anywhere-IPv4
12. Assign the security group to the load balancer:
    - Return to the browser tab where you are still configuring the load balancer.
    - In the Security groups area and choose the  refresh icon.
    - For Security groups, select the Inventory-LB security group you just created.
    - Next, below the Security groups dropdown menu, select the X next to the default security group so that Inventory-LB is now the only security group chosen.
13. In the Listeners and routing section, choose Create target group.A new browser tab will open.
14. Configure the target group as described here:
    - Choose a target type: Instances
    - Target group name: Inventory-App
    - VPC: Ensure that Lab VPC is chosen.
    - Scroll down and expand  Advanced health check settings. Note: The Application Load Balancer automatically performs health checks on all instances to ensure that they are responding to requests. The default settings are recommended, but you will make them slightly faster for use in this lab.
    - Healthy threshold: 2
    - Interval: 10 (seconds)
    - The configurations you have chosen will result in the health check being performed every 10 seconds, and if the instance responds correctly twice in a row, it will be considered healthy.
    - Choose Next. The Register targets screen appears. Note: Targets are the individual instances that will respond to requests from the load balancer.
    - You do not have any web application instances yet, so you can skip this step.
    - Review the settings and choose Create target group.
15. Return to the browser tab where you already started defining the load balancer.
16. In the Listeners and routing section, choose the refresh icon.
17. For the Listener HTTP:80 row, set the Default action to forward to the Inventory-App target group you just created.
18. Scroll to the bottom of the page, and choose Create load balancer.
    - The load balancer is successfully created.
    - Choose View load balancer.   

<p align="center">
<br/>
<img src="https://i.imgur.com/kdGGlp8.png"/>
 
<h2>Task 3: Creating an Auto Scaling Group</h2>

1. In the AWS Management Console, on the Services  menu, choose EC2.
2. In the left navigation pane, choose Instances.
3. Wait until the Status check for Web Server 1 displays 2/2 checks passed. Choose refresh to update.
4.Select  Web Server 1.
5. In the Actions  menu, choose Image and templates > Create image, then configure:
    - Image name: Web Server AMI
    - Image description: Lab AMI for Web Server
6. Choose Create image

<h2>Create a Launch Template and an Auto Scaling Group</h2>

1. In the left navigation pane, choose Launch Templates.
2. Choose Create launch template
3. Configure the launch template settings and create it:
    - Launch template name: Inventory-LT
    - Under Auto Scaling guidance, select  Provide guidance to help me set up a template that I can use with EC2 Auto Scaling
    - In the Application and OS Images (Amazon Machine Image) area, choose My AMIs.
    - Amazon Machine Image (AMI): choose Web Server AMI
    -  Instance type: choose t2.micro
    - Key pair name: choose vockey
    - Firewall (security groups): choose Select existing security group
    - Security groups: choose   Inventory-App
    - Scroll down to the Advanced details area and expand it.
    - IAM instance profile: choose Inventory-App-Role
    - Scroll down to the Detailed CloudWatch monitoring setting. Select  Enable Note: This will allow Auto Scaling to react quickly to changing utilization.
    - Under User data, paste in the script below:

<p align="center">
<br/>
<img src="https://i.imgur.com/40BNKis.png"/>

4. Choose Create launch template
5. In the Success dialog, choose the Inventory-LT launch template.
6. From the Actions menu, choose Create Auto Scaling group
7. Configure the details in Step 1 (Choose launch template or configuration):
    - Auto Scaling group name:  Inventory-ASG (ASG stands for Auto Scaling group)
    - Launch template: confirm that the Inventory-LT template you just created is selected.
    - Choose Next
8. Configure the details in Step 2 (Choose instance launch options):
    - VPC: choose Lab VPC
    - Availability Zones and subnets: Choose Private Subnet 1 and then choose Private Subnet 2.  
    - Choose Next
9. Configure the details in Step 3 (Configure advanced options):
    - In the Load balancing panel:
    - Choose Attach to an existing load balancer
    - Existing load balancer target groups: select Inventory-App.
    - In the Health checks panel:  Health check grace period: 90 seconds
    - In the Additional settings panel: Select  Enable group metrics collection within CloudWatch
    - This will capture metrics at 1-minute intervals, which allows Auto Scaling to react quickly to changing usage patterns.
    - Choose Next  
10. Configure the details in Step 4 (Configure group size and scaling policies - optional):
    - Under Group size, configure: 
    - Desired capacity: 2
    - Minimum capacity: 2
    - Maximum capacity: 2
    - Under Scaling policies, choose None 
    - Choose Next
