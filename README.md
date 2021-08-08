# geektime_homework04
## 1. 基本信息

学号: G20210735010190

## 2. 思路及运行截图
1. 作业一

```select tu.age age, avg(tr.rate) avgrate from t_user tu, t_rating tr where tu.userid=tr.userid and tr.movieid=2116 group by tu.age```

通过userid字段关联t_user表和t_rating表，然后选出movieid字段为2116的数据，根据age字段进行分组，然后查询tu.age字段并取别名为age，查询tr.rate字段根据分组的平均值并取别名avgrate。
![作业一代QueryId](https://user-images.githubusercontent.com/23160530/128646523-1569d36b-c0b7-476a-8cb6-6b02e182280b.png)

2. 作业二

```
with tur as 
(select tu.sex, tr.movieid movieid, avg(tr.rate) avgrate, count(tu.userid) total from t_user tu, t_rating tr where tu.userid=tr.userid and tu.sex="M" group by tr.movieid, tu.sex) 
select tur.sex sex, tm.moviename name, avgrate, total from tur, t_movie tm where tur.movieid=tm.movieid and total > 50 order by avgrate desc limit 10;
```

第一步，通过userid关联t_user表和t_rating表，并筛选sex字段为"M"的数据，根据movieid和sex字段进行分组，查询sex、movieid、rate的平均值及各组数据的计数值，并将结果保存为临时中间表tur。

第二步，通过movieid关联临时中间表tur和t_movie，筛选分组计数值大于50的数据，并根据rate平均值进行降序排序，选取前十个数据，查询tur.sex、tm.moviename、rate平均值、及计数值，并分别取别名为sex、name、avgrate、total。
![作业二带queryID](https://user-images.githubusercontent.com/23160530/128646955-02dc408e-f7c2-461f-ad9a-dd38c2f08e1e.png)

3. 作业三

```
with tur as 
(select tu.userid, count(1) sum from t_user tu, t_rating tr where tu.userid=tr.userid and tu.sex="F" group by tu.userid order by sum desc limit 1),
t_id_rate as 
(select movieid, rate from tur, t_rating where t_rating.userid=tur.userid order by rate desc, movieid limit 10),
t_id_avg as 
(select t_id_rate.movieid, avg(t_rating.rate) avgrate from t_id_rate, t_rating where t_id_rate.movieid=t_rating.movieid group by t_id_rate.movieid)
select t_movie.moviename `t.moviename`, t_id_avg.avgrate `t.avgrate` from t_id_avg, t_movie where t_movie.movieid=t_id_avg.movieid
```

第一步，通过userid关联t_user和t_rating表，筛选sex为"F"的数据，根据userid进行分组，同时根据各分组的计数值进行降序排列，选取计数值最大的数据，查询其userid以及计数值并保存为临时中间表tur即找到了评论最多的女士的userid。

第二步，通过userid关联tur表和t_rating表，根据rate进行降序排序，根据movieid进行升序排序，选取前十个数据，查询其movieid和rate字段并保存为临时中间表t_id_rate，即根据评论最多的女士的userid去影评表查询其给出最高平分的movieid及rate，由于该女士给59部电影打了5分最高分，所以为了保证查询唯一性，添加了对movieid进行升序排序。

第三步，通过movieid字段关联t_id_rate表和t_rating表，根据movieid进行分组，查询movieid及其平均评分，并将结果保存为临时中间表t_id_avg，即根据十部电影的movieid值获取影评表中的平均评分。

第四步，通过movieid字段关联t_id_avg表和t_movie表，查询moviename和avgrate字段，同时为了保持和作业要求截图一致，分别取别名为`t.moviename`和`t.avgrate`，即根据movieid获取对应的电影名，获得了最终结果电影名及对应平均评分。
![作业三带QueryId](https://user-images.githubusercontent.com/23160530/128647484-02b78b05-c474-4f09-bd75-82fd2d4a89d5.png)
