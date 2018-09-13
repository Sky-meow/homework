# 关于查询表内所有市和所有县区的思路总结
1.一开始的时候首先想到的是以前老师教的自链接查询
+ select a.cityName,b.cityName,c.cityName from s_provinces a,s_provinces b,s_provinces c 
where a.cityName="广东省" and a.id = b.parentId and b.id=c.parentId;
2. 但很明显，这种写法虽然很爽，但结构并不是很清晰，于是尝试用join
+ select x.id, p.cityName, s.cityName, x.cityName
from s_provinces x
       join s_provinces s on x.parentId = s.id
       join s_provinces p on s.parentId = p.id
where p.cityName = '广东省';