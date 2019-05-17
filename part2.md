# Part2

## SQL Server to Postgres **fullload and cdc** task
1. For source EC2 SQL Server rdp (remote desktop) using login **\administrator** and password **pAsswOrd12**
2. from the documentation https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.SQLServer.html find out the prerequisites for the EC2 SQL Server to use as CDC source.
3. Make the necessary changes to source sql server using rdp session and client ssms (SQLServer Management Studio)
4. Setup a new fullload and cdc task to migrate table **mytable** from sqlserver to aurora postgres.
5. run sample inserts on source sql server and verify data is being migrated to aurora postgres
6. additionally you can create update/delete statements to test the cdc.

```
--sample inserts
INSERT INTO mytable([fname],[lname]) VALUES('Ferris','Mccoy'),('Bradley','Britt'),('Raphael','Sampson'),('Lane','Gallagher'),('Jin','Marks'),('Marshall','Houston'),('Mannix','Mullen'),('Harper','Alvarez'),('Fuller','Mcneil'),('Denton','Petty');
INSERT INTO mytable([fname],[lname]) VALUES('Blake','Farley'),('Vladimir','Davenport'),('Roth','Rhodes'),('Yardley','Vazquez'),('Wang','Maldonado'),('Josiah','Colon'),('Mark','Head'),('Hilel','Koch'),('Linus','Hull'),('Solomon','Wells');
```

## Oracle to Postgres **fullload and cdc** task
1. For source RDS Oracle you can connect using EC2client ssh session and then connect using sh file oracleb.sh in the home folder.
2. from the documentation https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.Oracle.html find out the prerequisites for the RDS oracle to use as CDC source with logminer.
>hint: Source is RDS oracle and not a EC2 or Onprem Oracle instance, thus prerequisites will vary.
3. Make the necessary changes to source Oracle instance.
4. Setup a new fullload and cdc task to migrate table **MYTABLE** from oracle to aurora postgres.
5. Run sample inserts on source oracle and verify data is being migrated to aurora postgres
6. Additionally you can create update/delete statements to test the cdc.

```
--sample inserts

INSERT INTO mytable (id, EFFECTIVE_DATE, fname) VALUES (120, '04-NOV-00', 'Jason');
INSERT INTO mytable (id, EFFECTIVE_DATE, fname) VALUES (121, '04-NOV-99', 'Quinn');
INSERT INTO mytable (id, EFFECTIVE_DATE, fname) VALUES (122, '04-NOV-99', 'Malachi');
```
