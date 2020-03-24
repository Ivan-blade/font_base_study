+ 建表
```
    create database spj;
    use spj;
    CREATE TABLE IF NOT EXISTS `S` (
        `SNO` varchar(32) NOT NULL,
        `SNAME` varchar(32) DEFAULT NULL,
        `STATUS` int(12) DEFAULT NULL,
        `CITY` varchar(32) DEFAULT NULL,
        PRIMARY KEY (`SNO`)
    ) ENGINE = InnoDB DEFAULT CHARSET = utf8;
    INSERT INTO `S` SET SNO = "S1",SNAME = "精益",STATUS = 20,CITY = "天津";
    INSERT INTO `S` SET SNO = "S2",SNAME = "盛锡",STATUS = 20,CITY = "北京";
    INSERT INTO `S` SET SNO = "S3",SNAME = "东方红",STATUS = 20,CITY = "北京";
    INSERT INTO `S` SET SNO = "S4",SNAME = "丰太盛",STATUS = 20,CITY = "天津";
    INSERT INTO `S` SET SNO = "S5",SNAME = "为民",STATUS = 20,CITY = "上海";
    CREATE TABLE IF NOT EXISTS `P` (
        `PNO` varchar(32) NOT NULL,
        `PNAME` varchar(32) DEFAULT NULL,
        `COLOR` varchar(32) DEFAULT NULL,
        `WEIGHT` int(12) DEFAULT NULL,
        PRIMARY KEY (`PNO`)
     ) ENGINE = InnoDB DEFAULT CHARSET = utf8;
    INSERT INTO `P` SET PNO = "P1",PNAME = "螺母",COLOR = "红",WEIGHT = 12;
    INSERT INTO `P` SET PNO = "P2",PNAME = "螺栓",COLOR = "绿",WEIGHT = 17;
    INSERT INTO `P` SET PNO = "P3",PNAME = "螺丝刀",COLOR = "蓝",WEIGHT = 14;
    INSERT INTO `P` SET PNO = "P4",PNAME = "螺丝刀",COLOR = "红",WEIGHT = 14;
    INSERT INTO `P` SET PNO = "P5",PNAME = "凸轮",COLOR = "蓝",WEIGHT = 40;
    INSERT INTO `P` SET PNO = "P6",PNAME = "齿轮",COLOR = "红",WEIGHT = 30;
    CREATE TABLE IF NOT EXISTS `J` (
        `JNO` varchar(32) NOT NULL,
        `JNAME` varchar(32) DEFAULT NULL,
        `CITY` varchar(32) DEFAULT NULL,
        PRIMARY KEY (`JNO`)
    ) ENGINE = InnoDB DEFAULT CHARSET = utf8;
    INSERT INTO `J` SET JNO = "J1",JNAME = "三建",CITY = "北京";
    INSERT INTO `J` SET JNO = "J2",JNAME = "一汽",CITY = "长春";
    INSERT INTO `J` SET JNO = "J3",JNAME = "弹簧厂",CITY = "天津";
    INSERT INTO `J` SET JNO = "J4",JNAME = "造船厂",CITY = "天津";
    INSERT INTO `J` SET JNO = "J5",JNAME = "机车厂",CITY = "唐山";
    INSERT INTO `J` SET JNO = "J6",JNAME = "无线电厂",CITY = "常州";
    INSERT INTO `J` SET JNO = "J7",JNAME = "半导体",CITY = "南京";
    CREATE TABLE IF NOT EXISTS `SPJ` (
        `id` bigint(32) NOT NULL AUTO_INCREMENT,
        `SNO` varchar(32) DEFAULT NULL,
        `PNO` varchar(32) DEFAULT NULL,
        `JNO` varchar(32) DEFAULT NULL,
        `QTY` int(12) DEFAULT NULL,
        PRIMARY KEY (`id`)
    ) ENGINE = InnoDB DEFAULT CHARSET = utf8;
    INSERT INTO `SPJ` SET SNO = "S1",PNO = "P1",JNO = "J1",QTY = 200;
    INSERT INTO `SPJ` SET SNO = "S1",PNO = "P1",JNO = "J3",QTY = 100;
    INSERT INTO `SPJ` SET SNO = "S1",PNO = "P1",JNO = "J4",QTY = 700;
    INSERT INTO `SPJ` SET SNO = "S1",PNO = "P2",JNO = "J2",QTY = 100;
    INSERT INTO `SPJ` SET SNO = "S2",PNO = "P3",JNO = "J1",QTY = 400;
    INSERT INTO `SPJ` SET SNO = "S2",PNO = "P3",JNO = "J2",QTY = 200;
    INSERT INTO `SPJ` SET SNO = "S2",PNO = "P3",JNO = "J4",QTY = 500;
    INSERT INTO `SPJ` SET SNO = "S2",PNO = "P3",JNO = "J5",QTY = 400;
    INSERT INTO `SPJ` SET SNO = "S2",PNO = "P5",JNO = "J1",QTY = 400;
    INSERT INTO `SPJ` SET SNO = "S2",PNO = "P5",JNO = "J2",QTY = 100;
    INSERT INTO `SPJ` SET SNO = "S3",PNO = "P1",JNO = "J1",QTY = 200;
    INSERT INTO `SPJ` SET SNO = "S3",PNO = "P3",JNO = "J1",QTY = 200;
    INSERT INTO `SPJ` SET SNO = "S4",PNO = "P5",JNO = "J1",QTY = 100;
    INSERT INTO `SPJ` SET SNO = "S4",PNO = "P6",JNO = "J3",QTY = 300;
    INSERT INTO `SPJ` SET SNO = "S4",PNO = "P6",JNO = "J4",QTY = 200;
    INSERT INTO `SPJ` SET SNO = "S5",PNO = "P2",JNO = "J4",QTY = 100;
    INSERT INTO `SPJ` SET SNO = "S5",PNO = "P3",JNO = "J1",QTY = 200;
    INSERT INTO `SPJ` SET SNO = "S5",PNO = "P6",JNO = "J2",QTY = 200;
    INSERT INTO `SPJ` SET SNO = "S5",PNO = "P6",JNO = "J4",QTY = 500;
```
+ 查询
    + 求J1零件的SNO
        ```
            SELECT SNO,JNO FROM SPJ WHERE JNO = "J1";
        ```
    + 求供应工程J1零件P1的供应商号码SNO
        ```
            SELECT SNO,JNO,PNO FROM SPJ WHERE JNO = "J1" AND PNO = "P1";
        ```
    + 求供应工程J1零件为红色的供应商号码SNO
        ```
           SELECT SNO,PNO FROM SPJ WHERE JNO = "J1" AND PNO IN (SELECT PNO FROM P WHERE COLOR = "红"); 
        ```
    + 求没有使用天津供应商生产红色零件的工程号JNO
        ```
            SELECT DISTINCT JNO FROM SPJ WHERE JNO NOT IN (SELECT JNO FROM SPJ WHERE SNO IN (SELECT SNO FROM S WHERE CITY = "天津") AND PNO IN (SELECT PNO FROM P  WHERE COLOR = "红"));
        ```
    + 求至少用了供应商S1所供应的全部零件的工程号JNO
        ```
            SELECT DISTINCT JNO
            FROM SPJ SPJ1
            WHERE NOT EXISTS
            (SELECT * FROM SPJ SPJ2
            WHERE SPJ2.sno="S1" AND NOT EXISTS
            (SELECT * FROM SPJ SPJ3
            WHERE SPJ3.pno=SPJ2.pno
            AND SPJ3.jno=SPJ1.jno
            ));
        ```

+ 增删改
    + 把所有红色零件的颜色改成蓝色
        ```
            update P set COLOR = "蓝" where COLOR = "红";
        ```
    + 由S5供给J4的零件P6改为由S3供应，请做必要的修改
        ```
            UPDATE SPJ SET SNO = "S3" WHERE SNO = "S5" AND JNO = "J4";
        ```
    + 从供应商关系中删除S2的记录，并从供应情况关系中删除相应记录
        ```
            DELETE FROM S WHERE SNO = "S2";
            DELETE FROM SPJ WHERE SNO = "S2";
        ```
    + 请将（S2,J6,P4,200）插入供应情况关系 
        ```
            INSERT INTO SPJ (SNO,JNO,PNO,QTY) VALUES ("S2","J6","P4",200);
        ```
+ 视图
    + 为三建工程项目建立一个视图包括SNO,PNO,QTY,并完成以下查询
        ```
            CREATE VIEW JVIEW(SNO,PNO,QTY)
            AS
            SELECT SNO,PNO,QTY 
            FROM SPJ 
            WHERE JNO = "J1";
        ```
        + 找出三建工程项目中使用的各种零件代码以及数量
            ```
                SELECT PNO,QTY FROM JVIEW;
            ```
        + 找出供应商S1的供应情况
            ```
                SELECT * FROM JVIEW WHERE SNO = "S1";
            ```
    + 视图优点
        + 数据库视图允许简化复杂查询：数据库视图由与许多基础表相关联的SQL语句定义。 您可以使用数据库视图来隐藏最终用户和外部应用程序的基础表的复杂性。 通过数据库视图，您只需使用简单的SQL语句，而不是使用具有多个连接的复杂的SQL语句。

        + 数据库视图有助于限制对特定用户的数据访问。 您可能不希望所有用户都可以查询敏感数据的子集。可以使用数据库视图将非敏感数据仅显示给特定用户组。

        + 数据库视图提供额外的安全层。 安全是任何关系数据库管理系统的重要组成部分。 数据库视图为数据库管理系统提供了额外的安全性。 数据库视图允许您创建只读视图，以将只读数据公开给特定用户。 用户只能以只读视图检索数据，但无法更新。

        + 数据库视图启用计算列。 数据库表不应该具有计算列，但数据库视图可以这样。 假设在orderDetails表中有quantityOrder(产品的数量)和priceEach(产品的价格)列。 但是，orderDetails表没有一个列用来存储订单的每个订单项的总销售额。如果有，数据库模式不是一个好的设计。 在这种情况下，您可以创建一个名为total的计算列，该列是quantityOrder和priceEach的乘积，以表示计算结果。当您从数据库视图中查询数据时，计算列的数据将随机计算产生。

        + 数据库视图实现向后兼容。 假设你有一个中央数据库，许多应用程序正在使用它。 有一天，您决定重新设计数据库以适应新的业务需求。删除一些表并创建新的表，并且不希望更改影响其他应用程序。在这种情况下，可以创建与将要删除的旧表相同的模式的数据库视图