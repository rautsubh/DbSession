# Agenda

1. Create a *Replication Instance*.
2. Create Source and Target *Endpoints*.
3. Create a *Replication task* to migrate data from either a Oracle or SQLServer Source to Aurora Postgres target.


# Replication Instance
1. Open **AWS DMS Console** by clicking on link https://console.aws.amazon.com/dms/v2/home?region=us-east-1
2. Open **Replication Instances**.
3. Create a replication Instance with following Inputs:
    1. Name of instance and short description
    2. Select VPC (having name **myVPC**)
    3. Under advanced security and network configuration select “VPC Security Group” name having **EC2ClientSG**
    4. Lastly click on **Create**.

# Endpoints
Based on your interest you can work on a *Oracle to Aurora Postgres* Or a *SQL Server to Aurora Postgres* migration.

## SQL Server
1. Open **AWS DMS Console** by clicking on link https://console.aws.amazon.com/dms/v2/home?region=us-east-1
2. Open **Endpoints**.
3. Select Endpoint type as **Source endpoint**.
4. **Endpoint identifier**: a label for source sql server say **mssqlsource**
5. **Source engine**: Select *sqlserver*.
6. **Server name**: from the cloudformation stack output use the SQL Server endpoint e.g. *ec2-54-243-25-80.compute-1.amazonaws.com*
7. **Port**: *1433*
8. **User name**: *myuser*
9. **Password**: *pAsswOrd12*
10. **Database name**: *mydata*
11. Expand **Test endpoint connection (optional)**
12. **VPC**: Select VPC (having name *myVPC*)
13. **Replication instance**: select replication instance in previous section.
14. click on **Run test**
15. If the connection test fails troubleshoot the same.
16. Lastly click on **Create endpoint**.


## Oracle
1. Open **AWS DMS Console** by clicking on link https://console.aws.amazon.com/dms/v2/home?region=us-east-1
2. Open **Endpoints**.
3. Select Endpoint type as **Source endpoint**.
4. **Endpoint identifier**: a label for source Oracle  say **oraclesource**
5. **Source engine**: Select *oracle*.
6. **Server name**: from the cloudformation stack output use the Oracle endpoint e.g. *ro1elgnpepti0fd.cfbcererxz1y.us-east-1.rds.amazonaws.com*
7. **Port**: *1521*
8. **User name**: *myuser*
9. **Password**: *pAsswOrd12*
10. **Database name**: *mydata*
11. Expand **Test endpoint connection (optional)**
12. **VPC**: Select VPC (having name *myVPC*)
13. **Replication instance**: select replication instance in previous section.
14. click on **Run test**
15. If the connection test fails troubleshoot the same.
16. Lastly click on **Create endpoint**.


## Aurora Postgres
1. Open **AWS DMS Console** by clicking on link https://console.aws.amazon.com/dms/v2/home?region=us-east-1
2. Open **Endpoints**.
3. Select Endpoint type as **Target endpoint**.
4. **Endpoint identifier**: a label for target aurora cluster say **AuroraPGTarget**
5. **Source engine**: Select *aurora-postgres*.
6. **Server name**: from the cloudformation stack output use the Aurora Postgres **Writer** endpoint e.g. *rds-aupg106-111h2zti22ufc.cluster-cfbcererxz1y.us-east-1.rds.amazonaws.com*
7. **Port**: *5432*
8. **User name**: *myuser*
9. **Password**: *pAsswOrd12*
10. **Database name**: *mydata*
11. Expand **Test endpoint connection (optional)**
12. **VPC**: Select VPC (having name *myVPC*)
13. **Replication instance**: select replication instance in previous section.
14. click on **Run test**
15. If the connection test fails troubleshoot the same.
16. Lastly click on **Create endpoint**.
# Task (SQLServer -> Aurora Postgres)
1. Open **AWS DMS Console** by clicking on link https://console.aws.amazon.com/dms/v2/home?region=us-east-1
2. Open
# Task (Oracle -> Aurora Postgres)
1. Open **AWS DMS Console** by clicking on link https://console.aws.amazon.com/dms/v2/home?region=us-east-1
2. Open 
