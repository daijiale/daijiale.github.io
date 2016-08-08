title: 【SQL】复杂条件查询
date: 2016-07-29 11:24:32
tags:

 - SQL

 
categories:

 - 原创博文
 - Web开发笔记
 
---


>PS：SQL进行复杂条件运算，连表，内连，交集，并集等。

<!--more-->

# 正文

```php
        
        //设置查询区间为一天
        $unix_end_time = $unix_start_time+86400;

        //查询当天下单并且已购买的用户
        $condition1 = "SELECT mobile,total_amount FROM $this->_order_table  WHERE create_time > $unix_start_time AND create_time <=$unix_end_time AND status = 4";

        //查询当天下单并且未购买的用户
        $condition2 = "SELECT mobile,pid,total_amount FROM $this->_order_table  WHERE create_time > $unix_start_time AND create_time <=$unix_end_time AND status != 4 " ;

        //取当天下单并且已购买和当天下单未购买的用户交集,得到同一手机号重复下单用户
        $condition3 = "SELECT table2.mobile FROM ($condition1) AS table1 INNER JOIN ($condition2)AS table2 ON table1.mobile = table2.mobile";

        //取$condition2 和 $condition3 的差集,得到下单未购买的发券精准用户,并取所下单金额的最大值
        $condition = "SELECT table3.mobile,pid,MAX(table3.total_amount) FROM ($condition2) AS table3 WHERE table3.mobile NOT IN ($condition3) GROUP BY table3.mobile";

        $result = $this->_db->query($condition);

```


# 参考文献

- [w3cSchool_SQL](http://www.phpstudy.net/e/sql/)

- [实例SQL 简单，复杂查询，基本函数查询](http://www.cnblogs.com/zerocc/archive/2011/08/31/2161383.html)

- [SQL多表连接查询与集合的并、交、差运算查询](http://blog.csdn.net/woshisap/article/details/7348676/)
 

 
 

 
 
 
 
 
 
 
