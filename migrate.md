#�ָ���ܽ�
##��ʵʹ�õĶ��ǹ����ָͨ��join��select�����ݲ�ѯ����������ʹ��create as�﷨�����ݷָ����:
1.�ֲ�����
+ ���ȣ�����Ҫ��Ū��������Ҫ��������ʲô
+ select count(*) from s_provinces where depth=2 �������ǽ��м������ݲ�ѯ���������������˻��ɽ��ؼ���ѯ��������������ز���
+ ��α������ǶԱ�ĸ�ʽҪ��
+ create table lagou_city as
  select a.id, b.cityName as province, c.cityName as city, a.cityName as district from
  (select * from s_provinces where depth=3) a
    join s_provinces c on d.parentId = c.id and c.depth=2
    join s_provinces b on c.parentId = b.id and b.depth=1;
+ ��Ϊ������ԭ���ı����Ǵ���districtΪnull�����ݵģ������ں���Ĳ������ʱ��������
+ insert into lagou_city
  select c.id, p.cityName as province, c.cityName as city, null as district from (select * from s_provinces where depth=2) c
  join s_provinces p on c.parentId = p.id and p.depth = 1;
+ ���������Ǿ��ܺܿ�Ľ����ݷ�����������ҷ�����������ĸ�ʽ

2.union����������ʵ������Ĵ�ͬС�죬ֻ�������������һ��ִ�У�����˵Ч�ʽϵ�
+ create table lagou_city as
select d.id, p.cityName as province, c.cityName as city, d.cityName as district from
  (select * from s_provinces where depth=3) d
    join s_provinces c on d.parentId = c.id and c.depth=2
    join s_provinces p on c.parentId = p.id and p.depth=1
union
select c.id, p.cityName as province, c.cityName as city, null as district from (select * from s_provinces where depth=2) c
  join s_provinces p on c.parentId = p.id and p.depth = 1;
+ union�����ñ��ǽ��ṹ��ͬ�ı�ϲ���һ����������Ҳ�Ͳ���Ҫʹ�õ�insert�����
+ ��������ʹ�õĶ���join�﷨���ڽ���Ŀ����϶�����ˣ����������Ĺ����ϣ�����������ԭ���ϲ����������õ���update��join��䣬��ʽ�磺
+ UPDATE lagou_position  as a INNER JOIN lagou_city as b ON (a.district=b.district and a.city=b.city)
or (a.city=b.city and a.district is null and b.district is null)
SET a.adress_id=b.id;

+ ��������õ������޸Ĺ�������ʱ�ģ�Ϊ�˸����ٵ�Ѫ���ִ�У������ù����ָ����ǡ����

