# Impala Hands on
## 1. Using ```impala-shell -q``` option run sql command and using ```impala-shell -f``` run .sql file
### 1.1 Use impala ```-q``` query
```sh
impala-shell -q "select * from DBname.TableName limit 10;"
```
### 1.2 Import .sql file to execute query
#### 1.2.1 Write hql command to "impala_query.sql" file 
```sh
echo "select * from DBname.TableName limit 10;" >> impala_query.sql
# or you can write something by edit tool such as: vim, vi, nano
vim impala_query.sql
# Write something here
```
#### 1.2.2  Execute sql query from query.sql 
```sh
impala-shell -f impala_query.sql
```
## 2. Run Impala query
### 2.1 Goto impala-shell cli query
```sh
$ impale-shell
[ethonwumasternode:21000] > show databases;
[ethonwumasternode:21000] > select * from default.partquefuletable limit 10;
```
### 2.2 Run OS command in impala-shell cli
```sh
[ethonwumasternode:21000] > !ls -al;
```
## 3. Create DB , tables , Instert 
### 3.1 Create Database
```sh
[ethonwumasternode:21000] > create database <DBname>
-- Check successful create or not
[ethonwumasternode:21000] > show databases;
```
### 3.2 Create table
#### 3.2.1 Create table on default path, this command is store default on ```hdfs://user/hive/warehouse/DBname.db/tablename```
```sh
# Check you want to create table from which  DB, you should enter that
[ethonwumasternode:21000] > use YourDBname;
[ethonwumasternode:21000] > create table training (
		            customer_id int,
			    spending int,
			    define string)
			    row format delimited 
		     	    fields terminated by ',';
```
#### 3.2.2 If you want to create table and store on another path
```sh
[ethonwumasternode:21000] > use YourDBname;
[ethonwumasternode:21000] > create table training (
			    customer_id int,
		   	    spending int,
			    define string)
			    row format delimited 
			    fields terminated by ','
			    location '<Your_HDFS_dis_path>';
-- Check create sucessful in HDFS or not
[ethonwumasternode:21000] > exit;
$ hadoop dfs -ls '<Your_HDFS_dis_path>'
```
### 3.3 Insert data to table
```sh
--If want to insert one record:
[ethonwumasternode:21000] > insert into table YourDBname.training values (1,90,'Buy lunch box')
--If want to insert more than one record, just use "," to separat each record
[ethonwumasternode:21000] > insert into table YourDBname.training values (1,90,'Buy lunch box'),(2,150,'Buy a dozen of beer');
```
### 3.4 Create table based on EXISTING data
```sh
--If you want to create a table which schema is same as some table in Hive
[ethonwumasternode:21000] > create table "<New_Table_Name>" like "<same_as_some_table_name>"
```
### 3.5 Create table based on a SELECT
```sh
[ethonwumasternode:21000] > create table <YourTableName> as
                            -- Query hql command here
     			    select ??,??,??
     		            from YourDB.YourTable
        		    .... 
```
## 4. Import CSV to tables
### 4.1 If want to import CSV, first you must put your CSV to HDFS
```sh
hadoop dfs -put '<Your_CSV_OS_path>' '<Your_HDFS_dis_path>'
```
### 4.2 Create a table schema same as CSV 
```sh
[ethonwumasternode:21000] > use YourDBname;
[ethonwumasternode:21000] > create table YourTableName(
			    --Enter CSV schema here
			    )
			    row format delimited
			    fields terminated by ',';
```
### 4.3 Import CSV to Hive table
#### 4.3.1 None overwrite
```sh
[ethonwumasternode:21000] > load data inpath '<Your_HDFS_dis_path>' into table YourDBname.YourTableName;
```
#### 4.3.2 Overwrite, mean if this table already had data, it will recover that 
```sh
[ethonwumasternode:21000] > load data inpath '<Your_HDFS_dis_path>' overwrite into table YourDBname.YourTableName;
```
