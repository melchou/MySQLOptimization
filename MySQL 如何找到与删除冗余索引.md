1.如何跟踪MySQL中的冗余索引
`select * from sys.schema_unused_indexes;`

2.如何在MySQL中删除冗余索引

```
drop index `index_name` on table;
```
