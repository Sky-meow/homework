# ���ڲ�ѯ���������к�����������˼·�ܽ�
1.һ��ʼ��ʱ�������뵽������ǰ��ʦ�̵������Ӳ�ѯ
+ select a.cityName,b.cityName,c.cityName from s_provinces a,s_provinces b,s_provinces c 
where a.cityName="�㶫ʡ" and a.id = b.parentId and b.id=c.parentId;
2. �������ԣ�����д����Ȼ��ˬ�����ṹ�����Ǻ����������ǳ�����join
+ select x.id, p.cityName, s.cityName, x.cityName
from s_provinces x
       join s_provinces s on x.parentId = s.id
       join s_provinces p on s.parentId = p.id
where p.cityName = '�㶫ʡ';