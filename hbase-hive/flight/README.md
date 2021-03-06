# Hbase

## Open Hbase
Type  in cloudera terminal to open hbase
```bash
hbase shell
```
## List all tables 
```bash
list
```
## Creating a Table
```bash
#create <table_name> <column_family>
```
for example - 
```bash
create 'flight', 'info', 'schedule'
```
```bash
put 'flight',1, 'info:source', 'Pune'
put 'flight',1, 'info:destination', 'Mumbai'
put 'flight',1, 'schedule:departure', '10:32'
put 'flight',1, 'schedule:arrival', '11:20'
```
```bash
put 'flight',2, 'info:source', 'Nagpur'
put 'flight',2, 'info:destination', 'Pune'
put 'flight',2, 'schedule:departure', '13:42'
put 'flight',2, 'schedule:arrival', '14:37'
```
## Display Records
```bash
scan 'flight'
```
## Alter Table
Add one more column family to the table

```bash
alter 'flight', NAME => 'price'
```
Add records in altered table

```bash
put 'flight' , 1, 'price:single', '2000'
put 'flight' , 1, 'price:round', '3000'
put 'flight' , 2, 'price:single', '6000'
put 'flight' , 2, 'price:round', '11500'
```
To check changes

```bash
scan 'flight'
```
## Read data 

From a row, given a key :
```bash
get 'flight', 1
```

From a single column and single row :
```bash
get 'flight', '1' , COLUMN=>'info:source'
```
From multiple columns and single row :
```bash
get 'flight', '1' , COLUMN=>['info:source', 'info:destination']
```

## Delete Column Family
```bash
alter 'flight', NAME => 'price', METHOD=> 'delete'
```
To check changes

```bash
scan 'flight'
```

## To Disable a table
```bash
disable 'flight'
```

## To drop a table
```bash
drop 'flight'
```

# Hive

## From the Hive shell, join Hbase table

    > CREATE EXTERNAL TABLE hive_flight(fno int, fsource string, fdestination string, fsingle string, fround string)
    > STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    > WITH SERDEPROPERTIES ("hbase.columns.mapping" = ":key, info:source, info:destination, price:single, price:round")
    > TBLPROPERTIES("hbase.table.name" = 'flight');
## Display 
    > SELECT * FROM hive_flight
    
## Sum and Average

Note that fdelay should be an integer 
      
    > SELECT sum(fdelay) FROM hive_flight;
    > SELECT avg(fdelay) FROM hive_flight;
    
# Index

    > CREATE INDEX hive_flight_index
    > ON TABLE hive_flight(fno)
    > AS 'org.apache.hadoop.hive.ql.index.compact.CompactIndexHandler'
    > WITH DEFERRED REBUILD
    > ;
    
 To display the index 
 
    > SHOW INDEX ON hive_flight;





