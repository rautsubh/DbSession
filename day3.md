# Challenge

## 1A. Oracle to Postgres dms task challenge
1. Make sure we have taken care of prerequisites for the RDS oracle to use as CDC source with logminer https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.Oracle.html
>hint: Source is RDS oracle and not an EC2 or On-prem Oracle instance, thus prerequisites will vary.
2. Open **AWS DMS Console** by clicking on link https://console.aws.amazon.com/dms/v2/home?region=us-east-1
3. Open **Database migration tasks**.
4. Click on **Create task** from right top corner.
5. **Task Identifier**: *ORA2AUPGcdc*
6. **Replication instance** Select the replication instance.
7. **SourceDatabaseEndpoint** Select Oracle endpoint *oraclesource*.
8. **TargetDatabaseEndpoint** Select Aurora Postgres endpoint *aurorapgtarget*.
9. Set the Migration type to *Migrate existing data and replicate ongoing changes*
10. Under **Task settings** check the logging by selecting **Enable CloudWatch logs**.
11. Under **Table mappings** select table **NEWTABLE** from **MYUSER** schema.
12. Add a **transformation rule**
    1. to rename schema from MYUSER to public.
    2. convert the table NEWTABLE to lowercase.
    3. convert all column names (%) of table NEWTABLE to lowercase
13. Lastly **Create task**
14. Once the task is running in state **ongoing replication**, verify the Aurora Postgres Table newtable has required full load data:
```SQL
select * from newtable;
```
15. Run sample inserts/update statements to the source table
```SQL
INSERT INTO newtable (id,fname,lname,region) VALUES (3,'Arthur','Coleman','NORTH');
INSERT INTO newtable (id, fname,lname,region) VALUES (4, 'Nigel','Newman','SOUTH');
UPDATE newtable set region='SOUTH' where id=2;
```
16. Verify the data is getting migrating correctly.
17. If not, troubleshoot and fix the same.
18. Finally, do upload the task logs/source and target table (newtable) select outputs.
19. Compress both DMS and Aurora logs in a file with your name and Upload logs on day3logs.md


## 1B. SQLServer to Postgres dms task challenge
1. Make sure we have taken care of prerequisites for the EC2 SQLServer to use as CDC source with replication https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.SQLServer.html
2. For source EC2 SQL Server rdp (remote desktop) using login **\administrator** and password **pAsswOrd12**
3. Run PSQL script on target aurora postgres cluster as mentioned in next section:
2. Open **AWS DMS Console** by clicking on link https://console.aws.amazon.com/dms/v2/home?region=us-east-1
3. Open **Database migration tasks**.
4. Click on **Create task** from right top corner.
5. **Task Identifier**: *SQL2AUPGcdc*
6. **Replication instance** Select the replication instance.
7. **SourceDatabaseEndpoint** Select SQLserver endpoint *mssqlsource*.
8. **TargetDatabaseEndpoint** Select Aurora Postgres endpoint *aurorapgtarget*.
9. Set the Migration type to *Migrate existing data and replicate ongoing changes*
10. Under **Task settings** make sure that
    1. The **Target table preparation mode** is set to **Truncate**.
    2. Check the logging by selecting **Enable CloudWatch logs**.
11. Under **Table mappings** migrate table **finaltb** from **dbo** schema.
12. Add a **transformation rule**
    1. to rename schema from dbo to public.
13. Lastly **Create task**
14. Once the task is running in state **ongoing replication**, verify the Aurora Postgres Table newtable has required full load data:
```SQL
select * from finaltb;
```
16. Verify the data is getting migrating correctly.
17. If not, troubleshoot and fix the same.
18. Finally, do upload the task logs/source and target table (finaltb) select outputs.
19. Compress both DMS and Aurora logs in a file with your name and Upload logs on day3logs.md

# SQLServer to Postgres dms task script to run on target Aurora instance
Script to run on target Aurora Writer PSQL connection:

```SQL
CREATE TABLE finaltb(
microid BIGINT NOT NULL GENERATED ALWAYS AS IDENTITY( START WITH 1 INCREMENT BY 1),
description VARCHAR(255),
orderno BIGINT NOT NULL
)
        WITH (
        OIDS=FALSE
        );

CREATE TABLE ordertb(
orderid BIGINT NOT NULL GENERATED ALWAYS AS IDENTITY( START WITH 1 INCREMENT BY 1),
fname VARCHAR(255),
lname VARCHAR(255)
)
        WITH (
        OIDS=FALSE
        );

ALTER TABLE finaltb
ADD CONSTRAINT pk_finaltb_933578364 PRIMARY KEY (microid);

ALTER TABLE ordertb
ADD CONSTRAINT pk__ordertb__0809337dd08f972e PRIMARY KEY (orderid);

ALTER TABLE finaltb
ADD CONSTRAINT fk_finaltb_finaltb_965578478 FOREIGN KEY (orderno)
REFERENCES ordertb (orderid)
ON UPDATE NO ACTION
ON DELETE NO ACTION;
```

# 2. Aurora Postgres Challenge

## Steps

1. Ensure that the Instance-Type for the Aurora Writer is **db.r4.large**
2. Copy this Bash script and save as day3apg.sh

    ```bash
    #!/bin/bash

    connstr='postgresql://USER:PASSWORD@CLUSTER-ENDPOINT/'

    psql ${connstr}mydata -c "CREATE DATABASE chall3;"

    for k in `seq 1 5000`;
    do
      psql ${connstr}chall3 -c "CREATE TABLE IF NOT EXISTS tblz${k}(a integer);" >/dev/null &
    done

    for l in `seq 1 10000`;
    do
      for k in `seq 1 300`;
      do
        j=$(shuf -i1-5000 -n1)

        psql ${connstr}chall3 -c "INSERT INTO tblz${j} SELECT a from generate_series(1,100) as q(a); DELETE FROM tblz${j};" &
      done
    done
    ```

3. Change USER / PASSWORD / CLUSTER-ENDPOINT accordingly

  ```bash
  connstr='postgresql://USER:PASSWORD@CLUSTER-ENDPOINT/'

  in my use-case becomes

  connstr='postgresql://myuser:pAsswOrd12@robins5-aupg106-zcxm22x29joa.cluster-c0jv787kmw8j.us-east-1.rds.amazonaws.com/'
  ```

4. In Session 1
   1. Run the above Script as given below:

      ```bash
      watch -n 1 bash day3apg.sh 2>/dev/null 2>&1
      ```

5. In Session 2
   1. Check number of AutoVacuum Sessions:

      ```SQL
      SELECT now(),query
      FROM pg_stat_activity
      WHERE query ILIKE '%autovacuu%'
        AND query NOT ILIKE '%pg_stat_activity%';

      ```

6. Cancel the script that was sun in Session 1 (to stop the workload).

## Challenge
You need to ensure that your Aurora Postgres cluster is able to VACUUM faster (i.e. more than 3 concurrent AUTOVACUUMs at any point in time)

For e.g. Output to Step 5 (above) could look like this:

```SQL
mydata=>
SELECT clock_timestamp(),query
FROM pg_stat_activity
WHERE query ILIKE '%autovacuu%'
  AND query NOT ILIKE '%pg_stat_activity%';

        clock_timestamp        |                   query
-------------------------------+--------------------------------------------
 2019-05-20 03:30:40.734583+00 | autovacuum: VACUUM ANALYZE public.tblz1763
 2019-05-20 03:30:40.734595+00 | autovacuum: VACUUM ANALYZE public.tblz1252
 2019-05-20 03:30:40.7346+00   | autovacuum: VACUUM ANALYZE public.tblz2229
 2019-05-20 03:30:40.734605+00 | autovacuum: VACUUM ANALYZE public.tblz1351
 2019-05-20 03:30:40.734612+00 | autovacuum: VACUUM ANALYZE public.tblz2244
 2019-05-20 03:30:40.734616+00 | autovacuum: VACUUM ANALYZE public.tblz2106
 2019-05-20 03:30:40.73462+00  | autovacuum: VACUUM public.tblz1242
 2019-05-20 03:30:40.734625+00 | autovacuum: VACUUM ANALYZE public.tblz1286
 2019-05-20 03:30:40.73463+00  | autovacuum: VACUUM ANALYZE public.tblz1421
 2019-05-20 03:30:40.734634+00 | autovacuum: VACUUM ANALYZE public.tblz1174
 2019-05-20 03:30:40.734639+00 | autovacuum: VACUUM ANALYZE public.tblz2245
 2019-05-20 03:30:40.734643+00 | autovacuum: VACUUM ANALYZE public.tblz2263
 2019-05-20 03:30:40.734651+00 | autovacuum: VACUUM ANALYZE public.tblz2240
 2019-05-20 03:30:40.734656+00 | autovacuum: VACUUM ANALYZE public.tblz1757
 2019-05-20 03:30:40.734663+00 | autovacuum: VACUUM ANALYZE public.tblz1492
 2019-05-20 03:30:40.734668+00 | autovacuum: VACUUM ANALYZE public.tblz2193
 2019-05-20 03:30:40.734672+00 | autovacuum: VACUUM ANALYZE public.tblz2240
 2019-05-20 03:30:40.734679+00 | autovacuum: VACUUM ANALYZE public.tblz2222
 2019-05-20 03:30:40.734683+00 | autovacuum: VACUUM ANALYZE public.tblz1660
 2019-05-20 03:30:40.73469+00  | autovacuum: VACUUM ANALYZE public.tblz2278
 2019-05-20 03:30:40.734698+00 | autovacuum: VACUUM ANALYZE public.tblz2267
 2019-05-20 03:30:40.734703+00 | autovacuum: VACUUM ANALYZE public.tblz1148
 2019-05-20 03:30:40.734708+00 | autovacuum: VACUUM ANALYZE public.tblz2237
 2019-05-20 03:30:40.734713+00 | autovacuum: VACUUM ANALYZE public.tblz2843
 2019-05-20 03:30:40.734718+00 | autovacuum: VACUUM ANALYZE public.tblz4731
 2019-05-20 03:30:40.734723+00 | autovacuum: VACUUM ANALYZE public.tblz2559
 2019-05-20 03:30:40.734728+00 | autovacuum: VACUUM ANALYZE public.tblz2269
 2019-05-20 03:30:40.734732+00 | autovacuum: VACUUM ANALYZE public.tblz2587
 2019-05-20 03:30:40.734737+00 | autovacuum: ANALYZE public.tblz1712
(29 rows)

```

### Once done, *don't forget* to upload us the output of 5 consecutive runs of Step 5 similar to earlier log uploads.
### Compress both DMS and Aurora logs in a file with your name and Upload logs on day3logs.md
