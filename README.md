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
    - VPC: Remove the default VPC by choosing the X to the right of it. Then select  Lab VPC.
10 
