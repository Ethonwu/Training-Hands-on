# Hive Hands on
## 1. Use hive -e option run sql command and run .hql
### 1.1 Using hive -e query 
```sh
hive -e "select * from DBname.TableName limit 10;"
```
### 1.2 Import .hql file to execute query
#### 1.2.1 Write hql command to "query.hql" file 
```sh
echo "select * from default.partquefuletable limit 10;" >> query.hql
# or you can write something by edit tool such as: vim, vi, nano
vim query.hql 
# Write something here
```
#### 1.2.2  Execute hql query from query.hql 
```sh
hive -f query.hql
```
## 2. Goto hive and query
### 2.1 Goto hive cli mode 
```sh
$ hive
hive> show databases;
hive> select * from default.partquefuletable limit 10;

```
### 2.2 Run OS command in hive cli
```sh
hive> !ls -al
```
## 3. Create DB , tables , Instert 
### 3.1 Create Database
```sh
hive> create database <DBname>
-- Check successful create or not
hive> show databases;
```
### 3.2 Create table
#### 3.2.1 Create table on default path, this command is store default on ```hdfs://user/hive/warehouse/DBname.db/tablename```
```sh
# Check you want to create table from which  DB, you should enter that
hive> use YourDBname;
hive> create table training (
customer_id int,
spending int,
define string)
row format delimited 
fields terminated by ',';
```
#### 3.2.2 If you want to create table and store on another path
```sh
hive> use YourDBname;
hive> create table training (
customer_id int,
spending int,
define string)
row format delimited 
fields terminated by ',';
location '<Your_HDFS_dis_path>'
-- Check create sucessful in HDFS or not
hive> exit;
$ hadoop dfs -ls '<Your_HDFS_dis_path>'
```
### 3.3 Insert data to table
```sh
--If want to insert one record:
hive> insert into table YourDBname.training values (1,90,'Buy lunch box')
--If want to insert more than one record, just use "," to separat each record
hive> insert into table YourDBname.training values (1,90,'Buy lunch box'),(2,150,'Buy a dozen of beer');
```
### 3.4 Create table based on EXISTING data
```sh
--If you want to create a table which schema is same as some table in Hive
hive> create table "<New_Table_Name>" like "<same_as_some_table_name>"
```
### 3.5 Create table based on a SELECT
```sh
hive> create table <YourTableName> as
     # Query hql command here
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
hive> use YourDBname;
hive> create table YourTableName(
--Enter CSV schema here
)
row format delimited
fields terminated by ',';
```
### 4.3 Import CSV to Hive table
#### 4.3.1 None overwrite
```sh
load data inpath '<Your_HDFS_dis_path>' into table YourDBname.YourTableName;
```
#### 4.3.2 Overwrite, mean if this table already had data, it will recover that 
```sh
load data inpath '<Your_HDFS_dis_path>' overwrite into table YourDBname.YourTableName;
```
