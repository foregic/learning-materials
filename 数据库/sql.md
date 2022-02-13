````sql
# 提取每种性别的用户分别由多少参赛者
# 2138 |  180cm,75kg,27,male | http:/url/bigboy777
select substring_index(profile,',',-1) as gender,count(device_id)
from user_submit
group by gender
````

1、LOCATE(substr , str )：返回子串 substr 在字符串 str 中第一次出现的位置，如果字符substr在字符串str中不存在，则返回0；
2、POSITION(substr IN str )：返回子串 substr 在字符串 str 中第一次出现的位置，如果字符substr在字符串str中不存在，与LOCATE函数作用相同；
3、LEFT(str, length)：从左边开始截取str，length是截取的长度；
4、RIGHT(str, length)：从右边开始截取str，length是截取的长度；
5、SUBSTRING_INDEX(str ,substr ,n)：返回字符substr在str中第n次出现位置之前的字符串;
6、SUBSTRING(str ,n ,m)：返回字符串str从第n个字符截取到第m个字符；
7、REPLACE(str, n, m)：将字符串str中的n字符替换成m字符；
8、LENGTH(str)：计算字符串str的长度。

