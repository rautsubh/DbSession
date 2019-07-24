
# Setup Infrastructure

1. Open **AWS Console** by clicking on link https://console.aws.amazon.com/console/home?region=us-east-1
2. Under Services open EC2 (under group Compute)
3. From right side menu open **Key Pairs**
4. Create Key pair
    1. Give a name
    2. Save the pem file as generated which would be required later to SSH to ec2 client.
5. Open link http://bit.ly/2JJOG9G in a new window and copy the raw JSON
6. Open a text editor and paste the JSON contents and save file as *DBSession.json* locally.
7. Open Cloudformation console https://console.aws.amazon.com/cloudformation/home?region=us-east-1
8. Click on **Create Stack**
9. Under **Specify template** select the checkbox **Upload a template file**.
10. Upload template file *DBSession.json* from local drive.
11. Click **Next**.
12. Name the stack say *migrateInfra*.
13. From a new browser window open http://checkip.amazonaws.com/
14. Copy the IP address.
15. For Stack Parameters **ClientIP** enter the ipaddress followed by /32
    1. if my ip address is 49.180.175.238 then it would be 49.180.175.238/32
    2. if you want to connect from home or any other ip please set above address to 0.0.0.0/0
    3. For parameter **myKeyPair** select the keypair from the dropdown as created in initial steps.
17. Click **Next**.
18. Click **Next** (present at the end of the page) for the page **Configure stack options**
19. Under page **Review workshop01** scroll to end
20. Under **Capabilities** select the checkbox acknowledging **that AWS CloudFormation might create IAM resources.**
21. Lastly click on **Create Stack**.

# Stack Output
![Stack Output](https://github.com/rautsubh/DbSession/blob/master/stackOutput.png)
