### linux基础配置
#### yum源切换
+ 安装wget
```
    yum install -y wget
```
+ 备份/etc/yum.repos.d/CentOS-Base.repo文件
```
    cd /etc/yum.repos.d/
    mv CentOS-Base.repo CentOS-Base.repo.back
```
+ 下载阿里云的Centos-6.repo文件
```
    wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
    这边注意centos版本，如果不匹配的话，换了yum源可能更卡
```
+ 重新加载yum
```
    yum clean all
    yum makecache
```
#### centos7安装mysql8
+ 见菜鸟教程（以下是出了问题的补充）
+ 果不其然报错了
    + vim /var/log/mysqld.log（或者下面的命令）
    + cat /var/log/mysqld.log（系统提示的信息基本没用。。。直接看日志好吧）
    + 是权限问题
        + chmod -R 777 /var/lib/mysql
    + 下面又有登录问题了
        + 输入mysql报错如下
        + Access denied for user 'root'@'localhost' (using password: NO)（这个教程有毒。。。）
        + 主要是没有密码，所以密码在哪呢？
            + cat /var/log/mysqld.log（最上面会有一个临时密码，待会重写一下教程吧）
+ linux安装mysql8（流畅版）
    + 先按最上面的步骤换个yum源
    + 找到自己需要的版本注意系统版本centos7就选择linux7：https://dev.mysql.com/downloads/repo/yum/ 一般默认是最新的
    + 找到之后包名下面就是需要通过wget获取的字段，粘贴到wget http://repo.mysql.com/后面就好，如下（8.0.19为例）
    + wget http://repo.mysql.com/mysql80-community-release-el7-3.noarch.rpm
    + rpm -ivh mysql80-community-release-el7-3.noarch.rpm
    + yum update
    + yum install mysql-server
    + chown mysql:mysql -R /var/lib/mysql（用户设置--话说怎么会有mysql用户的？？？明明没有创建啊，跟着做就行）
    + chmod -R 777 /var/lib/mysql（给权限）
    + mysqld --initialize --console（这边参考了菜鸟window命令，可能会输出初始化密码，如果不行，删掉--console到时候在日志里查看就好了）
    + systemctl start mysqld（启动mysql服务）
    + systemctl status mysqld（查看mysql状态）
    + mysqladmin --version（验证安装）
    + 连接
        + mysql -u root -p
        + 输入日志文件中的密码
    + 这密码很不方便，开始修改密码的操作
        + 首先需要设置免密登录
            + vi /etc/my.cnf
            + 然后将 skip-grant-tables 字段添加至底部
            + 重启服务 systemctl restart mysqld
        + 登录 
            + mysql -u root -p
            + 回车（或者键入密码）进入
            + use mysql;(切换数据库)
            + select host, user, authentication_string, plugin from user;（查看基本信息）
            + update user set authentication_string='' where user='root';（将密码设置为空）
            + exit;
        + 一系列操作
            + 注释或者删除/etc/my.cnf文件中的skip-grant-tables 
            + 重启服务systemctl restart mysqld
        + 进入mysql并设置密码   
            + mysql -u root -p
            + 回车进入
            + ALTER user 'root'@'localhost' IDENTIFIED BY '修改后密码';
            + ok了
