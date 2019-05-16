# Database Session
## Infrastructure setup:
Please refer to steps mentioned at https://github.com/rautsubh/DbSession/blob/master/Infrastructure.md

## Infrastructure components
Once the Cloudformation Stack completes successfully would launch following components
    1. Oracle Instance running on an RDS Infrastructure
    2. EC2 SQL Server Windows 2016 SQL Server 2016 SP2 ENT edition
    3. Aurora Postgres Cluster with single node.
    4. EC2 Client linux (amazon linux 2) which would be used to connect to source and targets database instances.
From Cloudformation Stack output you will get following information:

1. SSH Command for EC2 client from your laptop
    1. This would be used to connect to EC2 client having required clients to connect to source and target instances.
2. SQL Server endpoint (public DNS)
3. Oracle instance endpoint
4. Aurora Postgres Writer and Reader Endpoints.
