# Agenda

1. Create a replication instance
2. Create Source and Target Endpoints
3. Create a replication task to migrate data from either Oracle or SQLServer to Aurora Postgres


# Replication Instance
Open AWS Console in US-East-1 region and then open service Database Migration Service
Open Replication Instances
Create a replication Instance
Inputs:
Name of instance and short description
Select VPC (myVPC)
Under advanced security and network configuration select “VPC Security Group” name having EC2ClientSG
