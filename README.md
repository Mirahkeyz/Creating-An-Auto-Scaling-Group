# Creating-An-Auto-Scaling-Group

Scenario: Now after testing and moving a server over to EC2, Level Up Bank is looking to move its network to the cloud to improve scalability, reliability and security. The bank has decided to create a VPC along with public subnets, a load balancer and a auto scaling group of t2.micro instances with Apache installed on them.

Project Overview: This project has 3 tiers of difficulty. Foundational, Advanced and Complex. I will only be completing the first two while breaking down each tier’s tasks and steps.

# Tier 1: Foundational

The tasks for Tier 1 are as follows:

— Create a VPC with CIDR 10.10.0.0/16 (Without using the VPC & More option)
— Create three public subnets with 10.10.1.0/24 & 10.10.2.0/24 & 10.10.3.0/24
— Create an autoscaling group using t2.micro instances. All instances should have Apache installed on each instance with a custom web page. Ensure the autoscaling group is using the public subnets from #2.
— The autoscaling min and max should be 2 and 5.
— Create an Application Load Balancer to distribute traffic to the autoscaling group.
— Create web server security group that allows inbound traffic from HTTP from your Application Load Balancer. As a security precaution, no one should be able to use the public IP of your webserver to reach your website. All users should be using the Application Load Balancer.
— Create a load balancer security group that allows inbound traffic from HTTP from 0.0.0.0/0.
— Use the DNS URL of the Application Load Balancer in a browser to verify you can reach your site.

Let’s get started!!!

The first thing we need to do is create a VPC with the CIDR 10.10.0.0/16. Head over to the VPC console and click on Create VPC. Leave VPC only selected, provide a name then type in the CIDR 10.10.0.0/16, then click on Create VPC.

![Snipe 1](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/a5e9ebbd-3f70-42aa-90bf-9e70b825e840)

On the left hand side menu under Your VPC’s, click on Subnets. Then click on Create subnet. On this Create subnet screen, we are going to create 3 of them. (10.10.1.0/24 & 10.10.2.0/24 & 10.10.3.0/24). Make sure you select the right VPC before you continue

![Snipe 2](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/7d4927cb-0c92-4b10-aa8a-9545d53239d5)

Once you have finished configuring the subnets, scroll down and click Create subnet.

![Snipe 3](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/65933173-fcee-489f-9b0f-1acbaa5bb443)

After creating the subnets, we need to click on each one individually and click on Actions, Edit subnet setting, Enable auto-assign public IPV4 address.

![Snipe 4](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/cef882aa-871a-4e7b-b75a-fa264df7134d)

Now we need to create a route table to allow them to connect to an Internet gateway (which we have not created yet). Search for Internet Gateway and create one. (Do not forget to attach it to the VPC).

Now go to route tables and create one.

![Snipe 5](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/2d46d932-27d8-46b0-99b7-5b9dd7807286)

After the route table creation, click on Edit subnet associations and select all of them and click Save associations. Now click Edit routes add a route to the internet gateway and save changes.

![Snipe 6](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/cccb426b-498c-4dc0-bdd5-27265d89db1b)

The next thing we have to do is create the auto scaling group. We can navigate over to the ASG console however, we will not be able to proceed until we have a launch template. We can create one by clicking on Create a launch template

![Snipe 7](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/e43bc8bc-2981-4aec-9136-e98695a2e769)

![Snipe 8](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/cc84effa-fec3-4b99-a180-09de3ea22681)

For the Application and OS Image section, I will be using my own AMI which already has Apache installed as well as my customized website. For Instance type, I will be selecting t2.micro.

![Snipe 9](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/2196cb2a-93ff-457c-9723-138a41986e69)

Next, create a new keypair then select it from the drop down box. In the Network settings Subnet section, leave as default. We will have to create a security group that allows HTTP and I will also allow SSH.

![Snipe 10](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/38a466d1-3205-445c-86d5-c623e65887be)

![Snipe 11](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/0da5c056-d85d-4f11-b985-f2c926e0e756)

Expand Advanced network configuration and click Add a network interface. In this section, make sure you Enable Auto-assign public IP

![Snipe 12](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/8c0bc1d0-9d39-40a6-99bf-54891f44b03e)

Now scroll down to Advanced Configuration and in the User data section, enter the script: (This is to make sure that all new EC2 instances have updated packages and Apache)

![Snipe 13](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/67ccd0b4-ac59-4be7-a939-f3bc59e14235)

Now go ahead and click on Create launch template. In the Auto Scaling group screen, click on the refresh icon and select the launch template we just created and click Next.

![Snipe 14](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/819fb61e-ddfd-4888-a92d-ebf584b5b4f5)

Now click on Create launch template. In the Launch templates console, click on Actions then Create Auto Scaling group.

Provide a name and make sure the LT we created is selected. Then click Next.

In the next screen choose the VPC and all 3 subnets and click Next.

![Snipe 15](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/e8312c4e-8c65-4442-ac2a-eb06c8a6886c)

Now we are going to click on Attach to a new load balancer.

![Snipe 16](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/42f8c724-478f-45ff-b626-ac314172ba01)

In the Network mapping section, make sure the right subnets are there and select Create a target group.

![Snipe 17](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/9d8edf70-affe-4774-a07e-3c69afe786ae)

Scroll down to Health checks and check the box next to Turn on Elastic Load balancing health checks and click Next.

Now comes the group size. The min should be 2 and the max should be 5. I decided to enter 3 for Desired capacity to have 1 instance per subnet. Click on Next until we can create the auto scaling group.

![Snipe 18](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/c351ef07-710d-45b4-9aae-f605c105da62)

Please give it the auto scaling group a few minutes to finish provisioning the instances.

![Snipe 19](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/970543f2-a0fb-42e4-9df0-8ef631777aba)

Now if we check the EC2 console, we should see 3 instances. One in each subnet.

![Snipe 20](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/2c3986e7-c968-4981-8693-ead8224566df)

We can go grab the public IP of an instance to test connectivity.

![Snipe 21](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/f6ceb394-c2ac-4eb7-843b-f0290a651625)

We should be able to access the site from each of the 3 public IP’s.

According to the project tasks, we can only allow website access through the load balancer DNS URL and not allow through the instance public IP.

The next step is to remove that access from the instance then creating a security group that allows HTTP access from the load balancer.

Head over to the security groups console and create a Load Balancer SG allowing HTTP from 0.0.0.0/0.

![Snipe 22](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/b5bfd45e-abed-40ff-89d1-60f994ecacd8)

Now edit the Web server SG by removing 0.0.0.0/0 from the source and replace it with the Load balancer SG.

This will keep anyone from accessing the instances directly via their public IP while allowing HTTP access from the application load balancer.

Time to test the instance public IP….No one will be able to acess the instance directly anymore.

![Snipe 23](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/86f537ba-c89d-4423-ae09-96f34f7e49c4)

Now to grab the load balancer DNS URL…..I am able to access the site using the load balancer DNS URL.

![Snipe 24](https://github.com/Mirahkeyz/Creating-An-Auto-Scaling-Group/assets/134533695/4f80fab2-4683-432e-a4a6-7a42c06189e2)

















































