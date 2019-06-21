<center><h1 style="color:blue">MySQL基础</h1></center>

[TOC]

## 1. 数据库

<u>数据库</u>定义：==保存有组织的数据的容器==

<u>表</u>定义：==某种特定类型数据的结构化清单==

<u>列</u>定义:==表中的一个字段。所有表都是由一个或多个列组成的==

<u>行</u>定义：==表中的一个记录==

<u>主键</u>定义：==一列，其值能够唯一区分表中每个行==

## 2. 数据库基本指令

切换数据库：==use database_name==

显示所有的数据库：==show databases==

显示所有的表：==show tables==

显示所有的列：==show columns from database_name==

## 3.数据检索
- **select语句**
- **order by 排序**
  - 逆序 desc
  - 顺序 asc
- **where 过滤**
  - and(<u>优先级高</u>)
  - or（<u>优先级低</u>） 
  运算符：
|操作符|说明|
|---|---|
|=|等于|
|<>||
|!=||
|<=||
|>||
|>=||
|BWTWEEN...AND...|在指定的两个值之间|
- **空值检查**

  在创建表时，表设计人员可以指定其中的列是否可以不包含值。在一个列不包含值时，称其为包含空值NULL。

  > *NULL 无值，它与**字段包含0、空字符串或仅仅包含空格不同。***

​	取出为null的行，例：

       ```sql
  select * from table_name where col_name is null
       ```

- **IN 操作符**（功能和or相当）

  ```sql
  select * from table_name where col_name in (上限，下限)
  ```

  <u>优点</u>：可以包含其它select语句

- **not 操作符**

  > *not 在where子句中用来否定后跟条件的关键字。*

  <u>注意</u>：==MySQL支持使用NOT对in、between和exists子句取反。==

- **like操作符**

  - **%**  通配符：任意字符出现任意次数
  - **_**   通配符：任意字符出现一次

- **正则表达式**（**REGEXP操作符**）

  ```sql
  select * from table_name where col_name REGEXP '正则表达式'
  ```

- **创建计算字段**

  - **拼接字段**：==Concat()==

    实例：

    ![2019-06-21_172414](img/2019-06-21_172414.png)

  - **去除空格**：==RTrim（）、Trim（）、LTrim（）==

- **使用别名（as）**

## 4. 数据处理函数

- 文本处理函数

  |    函数     |       说明        |
  | :---------: | :---------------: |
  |   Left()    | 返回串左边的字符  |
  |  Length()   |   返回串的长度    |
  |  Locate()   | 找出串的一个子串  |
  |   Lower()   |  将串转换为小写   |
  |   LTrim()   | 去掉串左边的空格  |
  |   Right()   | 返回串右边的字符  |
  |   RTrim()   | 去掉串右边的空格  |
  |  Soundex()  | 返回串的SOUNDEX值 |
  | SubString() |  返回子串的字符   |
  |   Upper()   |  将串转换为大写   |

- 日期和时间处理函数

  |     函数      |           说明           |
  | :-----------: | :----------------------: |
  |   AddDate()   | 增加一个日期（天、周等） |
  |   AddTime()   | 增加一个时间（时、分等） |
  |   CurDate()   |       返回当前日期       |
  |   CurTime()   |       返回当前时间       |
  |    Date()     |                          |
  |  DateDiff()   |                          |
  |  Date_Add()   |                          |
  | Date_Format() |                          |
  |     Day()     |                          |
  |  DayOfWeek()  |                          |
  |    Hour()     |                          |
  |   Minute()    |                          |
  |    Month()    |                          |
  |     Now()     |                          |
  |   Second()    |                          |
  |    Time()     |                          |
  |    Year()     |                          |

  