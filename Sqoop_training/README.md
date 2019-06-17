# SQOOP Hands on
## 1. Using SQOOP to query DB server data
```sh
# If using sql server , should add --driver com.microsoft.sqlserver.jdbc.SQLServerDriver
# Using MariaDB example
sqoop eval --connect "jdbc:mysql://<Hostname>:<Port>/<DBname>" 
	   --username "<YourUsername>" 
	   --password "<YourPassword>" 
	   --query "<query command>"
```
## 2. Import DB server data
### 2.1 Import DB server data to HDFS default store in your OS user name's folder on HFDS ```/user/<OS name>```
```sh
sqoop import --connect jdbc:mysql://<Hostname>:<Port>/<DBname>
	     --username "<YourUsername>"
	     --password "<YourPassword>"
	     --table "<Tablename>" 
	     -m 1
```
### 2.2 If want to asign HDFS path, do not wnat use dwfault path
```sh
sqoop import --connect jdbc:mysql://<Hostname>:<Port>/<DBname>
	     --username "<YourUsername>"
	     --password "<YourPassword>"
	     --table "<Tablename>" 
	     --target-dir '<HDFS Path>'
	     -m 1
```
### 2.3 Import DB server data to Hive  
```sh
sqoop import --connect "jdbc:mysql://<Hostname>:<Port>/<DBname>" 
	     --username "<YourUsername>"
	     --password "<YourPassword>"
	     --table "<DBTableName>"
	     -m 1 
	     --create-hive-table 
	     --hive-import 
	     --hive-table "<Hive table name>"
```
### 2.4 Import DB server data by query result
```sh
sqoop import --connect "jdbc:mysql://<Hostname>:<Port>/<DBname>" 
	     --username "<YourUsername>" 
	     --password "<YourPassword>" 
	     -m 1 
	     --create-hive-table 
	     --hive-import 
	     --hive-table "<Hive table name>" 
	     --query "select <Something> from <DBname>.<TableName> where "<Some condition>" AND \$CONDITIONS"
```
## 3. Processing some dirty data such as "\n"
### 3.1 Use ```--hive-drop-import-delims``` to remove "\n", "r"
```sh
sqoop import --connect "jdbc:mysql://<Hostname>:<Port>/<DBname>"
	     --username "<YourUsername>" 
	     --password "<YourPassword>"
	     --table "<Tablename>"
	     -m 1 
	     --create-hive-table 
	     --hive-import 
             --hive-drop-import-delims  
             --hive-table "<Hive table name>"
```
## 4. Export data to DB server
### Important!!! You must know schema , like in Hive is fields terminated by " ", if use default sqoop is use by ","
```sh
sqoop export --connect "jdbc:mysql://<Hostname>:<Port>/<DBname>?useUnicode=true&characterEncoding=utf-8" 
	     --username "<YourUsername>" 
	     --password "<YourPassword>" 
	     --table "<Tablename>" 
	     --export-dir "<Export HDFS path>"
	     -m 1 
	     --fields-terminated-by ' '
```
