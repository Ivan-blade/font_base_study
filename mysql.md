### count和avg函数对于字段为null的元组处理
+ avg
    + avg函数在求平均值时将会忽略null字段而不是作为0处理
+ count
    + count(*)
        + 用于统计记录行数，和null值无关，只要存在这行记录就会计数
    + count(字段名)
        + 对特定有数据的行进行计算将会忽略null值

### 查询单表
+ 查询表中若干列（select语句逗号隔开）
+ 查询表中若干元组
    + DISTINCT（去掉重复行）
        ```
            SELECT DISTINCT * FROM TABLENAME;
        ```
    + 查询满足条件的语句
        + 符号
            ```
                =, >, <, >=, <=, !=, <>, !>, NOT+
            ```
        + 语句
            + BETWEEN AND / NOT BETWEEN AND（表示范围）
                ```
                    select * from stu where age between 20 and 23;
                ```
            + IN / NOT IN(in后面加数据集)
                ```
                    select  *  from  table  where   uname  in(select  uname  from  user);
                    select  *  from  table  where   uname  in("1","2","3","4");
                ```
            + LIKE / NOT LIKE
                + 你可以在 WHERE 子句中使用LIKE子句
                + 你可以使用LIKE子句代替等号 =
                + LIKE 通常与 % 一同使用，类似于一个元字符的搜索。
                    ```
                        LIKE '%a'     //以a结尾的数据
                        LIKE 'a%'     //以a开头的数据
                        LIKE '%a%'    //含有a的数据
                        LIKE '_a_'    //三位且中间字母是a的
                        LIKE '_a'     //两位且结尾字母是a的
                        LIKE 'a_'     //两位且开头字母是a的
                    ```
            + IS NULL / IS NOT NULL
                + 判断是否为空，不能用=代替IS
        + 多重条件（逻辑运算）
            + AND, OR, NOT
+ ORDER BY子句
    + 按一个或者多个属性列排序
    + 升序ASC（高到低）,降序DESC（低到高）
        + ASC排序空值最后显示（默认）
        + DESC排序空值最先显示
            ```
                select * from sc where cno = 3 order by sdept, sage DESC;
                按sdept升序，同一sdept中sage降序排列 
            ```
+ 聚集函数
    + 计数
        + COUNT ([DISTINCT|ALL]*)
        + COUNT ([DISTINCT|ALL]<col_name>)
    + 计算总和
        + SUM ([DISTINCT|ALL]<col_name>)
    + 计算平均值
        + AVG ([DISTINCT|ALL]<col_name>)
    + 最大最小值
        + MAX ([DISTINCT|ALL]<col_name>)
        + MIN ([DISTINCT|ALL]<col_name>)
+ GROUP BY(字句分组)
    + 细化聚集函数的作用对象
        + 按指定的一列或多列值分组，值相等的为一组（要考）
            ```
                SELECT * FROM employee_tbl;
                +----+--------+---------------------+--------+
                | id | name   | date                | singin |
                +----+--------+---------------------+--------+
                |  1 | 小明 | 2016-04-22 15:25:33 |      1 |
                |  2 | 小王 | 2016-04-20 15:25:47 |      3 |
                |  3 | 小丽 | 2016-04-19 15:26:02 |      2 |
                |  4 | 小王 | 2016-04-07 15:26:14 |      4 |
                |  5 | 小明 | 2016-04-11 15:26:40 |      4 |
                |  6 | 小明 | 2016-04-04 15:26:54 |      2 |
                +----+--------+---------------------+--------+

                SELECT name, COUNT(*) FROM   employee_tbl GROUP BY name;
                +--------+----------+
                | name   | COUNT(*) |
                +--------+----------+
                | 小丽 |        1 |
                | 小明 |        3 |
                | 小王 |        2 |
                +--------+----------+
            ```
        + 未对查询结果分组，聚集函数将作用与整个查询结果
        + 对查询结果分组后，聚集函数将分别作用与每个组
        + 作用对象是查询的中间结果表
+ HAVING 和 WHERE语句的区别
    + 当需要根据聚集函数获取相应数据时需要使用having进行筛选
        ```
            SELECT Sdept , COUNT(Sdept) FROM student GROUP BY Sdept HAVING COUNT(Sdept) >= 2;
        ```
    + where作用于基表或者视图，从中选择满足条件的元组
    + having作用于组，选择满足条件的组

+ 嵌套查询
    ```
        SELECT  Sno，Sname，Sdept    
         FROM  Student  WHERE ( SELECT Sdept FROM Student WHERE Sname= ‘ 刘晨 ’ ) = Sdept
    ```