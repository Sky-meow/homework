# 分割方法总结
## 其实使用的都是关联分割，通过join和select将数据查询出来，进而使用create as语法将数据分割出来:
1.分步操作
+ 首先，我们要先弄明白我们要的数据是什么
+ select count(*) from s_provinces where depth=2 如这句便是将市级的数据查询出来，而我们依此还可将县级查询出来进而进行相关操作
+ 其次便是我们对表的格式要求
+ create table lagou_city as
  select a.id, b.cityName as province, c.cityName as city, a.cityName as district from
  (select * from s_provinces where depth=3) a
    join s_provinces c on d.parentId = c.id and c.depth=2
    join s_provinces b on c.parentId = b.id and b.depth=1;
+ 因为在我们原本的表中是存在district为null的数据的，所以在后面的插入操作时需声明：
+ insert into lagou_city
  select c.id, p.cityName as province, c.cityName as city, null as district from (select * from s_provinces where depth=2) c
  join s_provinces p on c.parentId = p.id and p.depth = 1;
+ 这样，我们就能很快的将数据分离出来，并且符合我们所需的格式

2.union，这个语句其实与上面的大同小异，只不过将多条语句一起执行，但据说效率较低
+ create table lagou_city as
select d.id, p.cityName as province, c.cityName as city, d.cityName as district from
  (select * from s_provinces where depth=3) d
    join s_provinces c on d.parentId = c.id and c.depth=2
    join s_provinces p on c.parentId = p.id and p.depth=1
union
select c.id, p.cityName as province, c.cityName as city, null as district from (select * from s_provinces where depth=2) c
  join s_provinces p on c.parentId = p.id and p.depth = 1;
+ union的作用便是将结构相同的表合并在一起，这样我们也就不需要使用到insert语句了
+ 不过以上使用的都是join语法，在今天的课堂上都是如此，不过在最后的关联上，由于我是在原表上操作，所以用的是update，join语句，格式如：
+ UPDATE lagou_position  as a INNER JOIN lagou_city as b ON (a.district=b.district and a.city=b.city)
or (a.city=b.city and a.district is null and b.district is null)
SET a.adress_id=b.id;

+ 不过这个用的是在修改关联数据时的，为了更快速的血语句执行，还是用关联分割更加恰当。

