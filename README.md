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

Connecting from EC2 Client to Database instances:

After SSHing to the ec2 client either using terminal (mac) or Putty (windows).
Home folder would have following files
```
[ec2-user@ip-172-31-0-17 ~]$ ll
total 44
-rwxr-xr-x 1 root root   122 May 15 04:45 aupsqlrb.sh
-rwxr-xr-x 1 root root   119 May 15 04:45 aupsqlw.sh
-rwxr-xr-x 1 root root    84 May 15 04:45 mssqlb.sh
-rwxr-xr-x 1 root root   276 May 15 04:45 oracleb.sh
```

you can run this sh files to connect to different instances

e.g.
```
[ec2-user@ip-172-31-0-17 ~]$ ./mssqlb.sh
Version: 0.15.0
Mail: sqlcli@microsoft.com
Home: http://github.com/dbcli/mssql-cli
master> select getdate();
+-------------------------+
| (No column name)        |
|-------------------------|
| 2019-05-16 22:58:09.177 |
+-------------------------+
(1 row affected)
Time: 0.351s
master>
```
1. aupsqlrb.sh: this will connect to Aurora Postgres Cluster reader endpoint.
2. aupsqlw.sh: this will connect to Aurora Postgres Cluster writer endpoint.
3. mssqlb.sh: this will connect to SQL Server instance.
4. oracleb.sh: this will connect to Oracle Instance.
