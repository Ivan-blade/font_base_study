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
