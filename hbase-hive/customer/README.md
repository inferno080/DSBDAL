# Hive

## Add the three text files as tables

#### Customer

```
> CREATE EXTERNAL TABLE customer(cid int, cname string , oid int)
> ROW FORMAT DELIMITED FIELDS TERMINATED BY "," STORED AS TEXTFILE;
```
```
> LOAD DATA LOCAL INPATH '/home/cloudera/Desktop/customers.txt' INTO TABLE customer
```
```
> SELECT * FROM customer;
```
#### Orders

```
> CREATE EXTERNAL TABLE order(oid int, iid string , quantity int)
> ROW FORMAT DELIMITED FIELDS TERMINATED BY "," STORED AS TEXTFILE;
```
```
> LOAD DATA LOCAL INPATH '/home/cloudera/Desktop/orders.txt' INTO TABLE order
```
```
> SELECT * FROM order;
```

#### Items

```
> CREATE EXTERNAL TABLE item(iid int, iname string , price int)
> ROW FORMAT DELIMITED FIELDS TERMINATED BY "," STORED AS TEXTFILE;
```
```
> LOAD DATA LOCAL INPATH '/home/cloudera/Desktop/items.txt' INTO TABLE item
```
```
> SELECT * FROM item;
```
## Join
```
> SET hive.auto.convert.join=false;
```
```
> SELECT customer.cname, order.quantity, item.price FROM customer JOIN order ON (customer.oid = order.oid) JOIN item ON (order.iid = item.iid);
```

# Sum and Avg

```
> SELECT sum(item.price*order.quantity), avg(item.price*order.quantity) FROM customer JOIN order ON (customer.oid = order.oid) JOIN item ON (order.iid = item.cid);
```



