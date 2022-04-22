# SQL Cheat Sheet


Sources:
  * https://devhints.io/mysql




## CRUD operations


Update one or many fields:
```
UPDATE table1 SET field1=new_value1 WHERE condition;
UPDATE table1, table2 SET field1=new_value1, field2=new_value2, ... WHERE
  table1.id1 = table2.id2 AND condition;
```


