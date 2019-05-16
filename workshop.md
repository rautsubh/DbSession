# Agenda

1. Create a *Replication Instance*
2. Create Source and Target *Endpoints*.
3. Create a *Replication task* to migrate data from either Oracle or SQLServer to Aurora Postgres


# Replication Instance
1. Open **AWS DMS Console** by clicking on link https://console.aws.amazon.com/dms/v2/home?region=us-east-1
2. Open Replication Instances
3. Create a replication Instance with following Inputs:
    1. Name of instance and short description
    2. Select VPC (having name **myVPC**)
    3. Under advanced security and network configuration select “VPC Security Group” name having **EC2ClientSG**
    4. Lastly click on **Create**.
