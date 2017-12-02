# IdGenerator
交易场景下的唯一Id生成器

##介绍
可以单机产生分布式全局唯一Id,核心的方法为模仿Twitter snowflake。单机产生id的好处是避免了从数据库中获取序列码产生的传输问题，以及单点问题。
Id的组成如下：
 >* 1 ms级时间 41bit， 可以支持69年 
 >* 2 服务器标志 8bit， 可以支持255台机器的应用，如果机器数目过多，可以考虑缩短时间占用位置
 >* 3 流水码 6bit 0-63 仿Twitter snowflake ms内最多产生64的seq
 >* 4 前三项和加上第一个bit 0 一共56位，转换成long共17位
 >* 5 用户Id埋点2个字符 (userId: 23423423454) 54 交易场景下根据userId查询较多,方便分表
 >* 6 前三项已经可以产生全局唯一Id了，但是由于交易场景下按用户Id分库分表较多，所以在最后埋上用户Id信息，一共19位

##性能
>* CPU： 2.5 GHz Intel Core i5
>* 内存： 8 GB 1600 MHz DDR3
>* 单线程：每秒4W-5W
