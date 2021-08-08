# geektime_homework04
## 1. 基本信息

学号: G20210735010190

## 2. 思路及运行截图
1. 作业一

```select tu.age age, avg(tr.rate) avgrate from t_user tu, t_rating tr where tu.userid=tr.userid and tr.movieid=2116 group by tu.age```
通过userid字段关联t_user表和t_rating表，然后选出movieid字段为2116的数据，根据age字段进行分组，然后获取tu.age字段并取别名为age，获取tr.rate字段根据分组的平均值并取别名avgrate。
![作业一代QueryId](https://user-images.githubusercontent.com/23160530/128646523-1569d36b-c0b7-476a-8cb6-6b02e182280b.png)
2. 作业二

