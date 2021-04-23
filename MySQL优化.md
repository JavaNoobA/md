业务背景：

订单主表和订单详情表，主表和详情表数据都是 1w 不到

```mysql
select
xxx
from ysol_fy_order order
left join ysol_fy_order_ detail on order.id = detail.order_id
xxxx
```

![MySQL-left-join很慢](C:\Users\n\Downloads\MySQL-left-join很慢.png)

## Nested Loop Join算法

将外层表的结果集作为循环的基础数据，然后循环从该结果集每次一条获取数据作为下一个表的过滤条件去查询数据，然后合并结果。如果有多个表join，那么应该将前面的表的结果集作为循环数据，取结果集中的每一行再到下一个表中继续进行循环匹配，获取结果集并返回给客户端。

```java
for each row in t1 matching range {
  for each row in t2 matching reference key {
     for each row in t3 {
      if row satisfies join conditions,
      send to client
    }
  }
 }
```

参考：https://www.cnblogs.com/wanbin/p/9592943.html

最后只要在订单详情表order_id 加索引就解决了