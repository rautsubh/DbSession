# Agenda

1. Create a **Replication Instance**
2. Create Source and Target **Endpoints**.
3. Create a **Replication task** to migrate data from either Oracle or SQLServer to Aurora Postgres


# Replication Instance
1. Open **AWS Console** by clicking on link https://console.aws.amazon.com/console/home?region=us-east-1
2. Under Services open
Open AWS Console in US-East-1 region and then open service Database Migration Service
Open Replication Instances
Create a replication Instance
Inputs:
Name of instance and short description
Select VPC (myVPC)
Under advanced security and network configuration select “VPC Security Group” name having EC2ClientSG
