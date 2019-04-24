---
title: Mybatis高级查询之一对多查询笔记
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---
### 2.1 使用**collection**嵌套结果映射
 - 假设如下场景，一个用户有多个角色，一个角色有多个权限。
![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556087075229.png)
![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556087195816.png)
#### 2.1.1 与一对一查询的区别
 - 区别：只是**把association**改成了**colection**,并且**property**属性的值改为了**roleList**，这里很好理解因为一对多，多端肯定是以集合来存储的。
 - 简化写法
 ![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556087444361.png)
 - 接下来看**查询方法**如何写
 ![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556087633336.png)
 - 接下来设测试（略），设置断点查看如下
 ![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556087874973.png)
 - 我们查出来两个用户，第一个用户有两个角色。
 - 接下来看输出日志
 ![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556088051095.png)
 ### 2.1.2 结果自动合并的情况
 - SQL执行结果三条但是用户数最后却只有两个，也就是说collection处理后变成了两条。因为第一个用户又拥有两个角色，经过转换为一对多数据结构后就变成了两个，毋庸置疑的。理解这个**转换过程**至关重要。
 - 为什么会自动合并用户1的两条查询结果呢？因为用户表是通过ID来判断用户是否相同的。所以MyBatis合并的依据是在
 **在resultMap**中配置了**ID字段**。如下图所示：![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556088357996.png)
 - 一种极端的情况：如果我们没有在**resultMap**中配置**id**，那么mybatis就会把**resultMap**中所有的字段逐一比较，如果**全部相同则合并**，**存在一个不同不合并**。
 - 另一种极端情况我们**resultMap**中配置了ID，但是在**查询语句中**没有查询ID列，这时候就会导致**嵌套集合**只有一条数据。很好解释，因为没有查询ID所以导致对应的ID值为NULL，所以mybatis在比较的时候是根据ID来比较都是NULL所以合并成一条数据。
 - 下面我们继续，一个角色对应多个权限。增加如下
 ![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556089105687.png)
 ![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556089173343.png)
 ![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556089472899.png)
 - 说明
 - 上面省略了权限实体类。
 - 其次xml文件配置简易图示如下。
![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556090427447.png)
![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556090828536.png)
- 注意别名
### 2.2 使用**Collection集合的嵌套查询**
对比：我们在一对一查询的时候使用association的关联嵌套查询，分为主查询和从查询，所以配置的时候很简单。在一对多中Collection也存在嵌套查询。
- 首先我们在privilegeMapper.xml中加入下面方法，用来根据角色ID来查询用户所属权限。
![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556091846288.png)
- 然后我们roleMapper.xml ，我们可以看到**colection**标签中，与当初配置**一对一association嵌套查询一致**，增加了column、select属性。
![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556092149527.png)
- 同时我们增加一个查询方法用于根据用户ID查询用户的角色。
![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556092201841.png)
-然后我们配置顶层userMapper.xml
![enter description here](https://www.github.com/QuinnTian/imgchr/raw/master/imgs/1556092441346.png)
- 一对多的嵌套查询和一对一的嵌套查询一致，只不过用了不同的标签，有些配置可以参考一对一嵌套查询。