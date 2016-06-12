## MySQL5.6简介

``` 
ySQL 5.5发布两年后，Oracle宣布MySQL 5.6正式版发布，首个正式版版本号为5.6.10。在MySQL 5.5中使用的是InnoDB作为默认的存储引擎，而MySQL 5.6则对InnoDB引擎进行了改造，提供全文索引能力，使InnoDB适合各种应用场景。
About MySQL 5.6
InnoDB 性能改进
在线DDL更改
对InnoDB 进行NoSQL访问 
```

## MySQL 5.6 功能：

**MySQL 5.6 同时引入了 NoSQL 接口，提供了兼容 memcached 的 API，该特性让应用可直接访问 InnoDB 存储引擎。底层上保持着跟关系数据库引擎在维护上的统一。同时底层的 InnoDB 引擎也增强在持久化优化统计、多线程消除以及提供更多的系统表和监控数据。**

MySQL 的产品经理 Tomas Ulin 解释了开源社区对 Oracle 关于补丁政策的批评，他说：这是一个不断求证的过程，我们每三个月提供安全补丁，但其实大多数用户并不会这么频繁的更新。而使用社区版的用户抱怨 Oracle 没有提供发行说明中 CVE 号的详细说明，它们只是简单的指向 Oracle 内部的错误码。公司将不会发布这些详情信息。

##MySQL 5.6 显著提高了性能和可用性，可支持下一代 Web、嵌入式和云计算应用程序。

```
MySQL Database 5.6 具备以下特性：

新增！ 在线 DDL /更改数据架构支持动态应用程序和开发人员灵活性

新增！ 复制全局事务标识可支持自我修复式集群

新增！ 复制无崩溃从机可提高可用性

新增！ 复制多线程从机可提高性能

新增！ 对 InnoDB 进行 NoSQL 访问，可快速完成键值操作以及快速提取数据来完成大数据部署

改进！ 在 Linux 上的性能提升多达 230%

改进！ 在当今、多核、多 CPU 硬件上具备更高的扩展力

改进！ InnoDB 性能改进，可更加高效地处理事务和只读负载

改进！ 更快速地执行查询，增强的诊断功能

改进！ Performance Schema 可监视各个用户/应用程序的资源占用情况

改进！ 通过基于策略的密码管理和实施来确保安全性

高度可靠，几乎无需干预即可确保系统持续不间断运行

简便易用，只需 3 分钟即可完成从下载到开发环境的安装和配置过程

管理需求低，数据库维护工作非常少

复制功能 支持灵活的拓扑架构，可实现向外扩展和高可用性

分区 有助于提高性能和管理超大型数据库环境

ACID 事务 支持构建安全可靠的关键业务应用程序

存储过程 可提高开发人员效率
```

## RPM安装5.6 

**请参考链接：http://www.sysopen.cn/20160612/

## my.cnf 优化配置
```
[mysqld]
#mysqld程序--目录和文件
user=mysql                          # MySQL启动用户：mysql
datadir = /var/lib/mysql            #  使用给定目录作为根目录(安装目录)。
server-id = 1                       #表示是本机的序号为1,一般来讲就是master的意思
pid-file=/opt/mysql/log/mysql.pid    #为mysqld程序指定一个存放进程ID的文件(仅适用于UNIX/Linux系统);

socket=/var/lib/mysql/mysql.sock   # 为MySQL客户程序与服务器之间的本地通信指定一个套接字文件(Linux下默认是/var/lib/mysql/mysql.sock文件)
port  = 3306                       # 指定MsSQL侦听的端口
key_buffer       = 384M            # key_buffer_size指定用于索引的缓冲区大小，增加它可得到更好处理的索引(对所有读和多重写)，到你能负担得起那样多。如果你使它太大，系统将开始换页并且真的变慢了。对于内存在4GB左右的服务器该参数可设置为384M或512M。通过检查状态值Key_read_requests和 Key_reads,可以知道key_buffer_size设置是否合理。比例key_reads / key_read_requests应该尽可能的低，至少是1:100，1:1000更好(上述状态值可以使用SHOW STATUS LIKE ‘key_read%'获得)。注意：该参数值设置的过大反而会是服务器整体效率降低!
table_cache = 512                  #table_cache指定表高速缓存的大小。每当MySQL访问一个表时，如果在表缓冲区中还有空间，该表就被打开并放入其中，这样可以更快地访问表内容。通过检查峰值时间的状态值Open_tables和Opened_tables，可以决定>是否需要增加table_cache的值。如果你发现 open_tables等于table_cache，并且opened_tables在不断增长，那么你就需要增加table_cache的值了(上述状态值可以使用SHOW STATUS LIKE ‘Open%tables'获得)。注意，不能盲目地把table_cache设置成很大的值。如果设置得太高，可能会造成文件描述符不足，从而造成性能不稳定或者连接失败。


default-storage-engine=InnoDB      #innodb为默认数据引擎
max_connections = 1000000          #指定MySQL允许的最大连接进程数。如果在访问论坛时经常出现Too Many Connections的错误提示，则需要增大该参数值。
max_connect_errors = 100000000     #对于同一主机，如果有超出该参数值个数的中断错误连接，则该主机将被禁止连接。如需对该主机进行解禁，执行：FLUSH HOST;。
wait_timeout = 8                   #指定一个请求的最大连接时间，对于4GB左右内存的服务器可以设置为5-10。
thread_concurrency = 8             #该参数取值为服务器逻辑CPU数量×2，在本例中，服务器有2颗物理CPU，而每颗物理CPU又支持H.T超线程，所以实际取值为4 × 2 = 8

long_query_time = 1                #慢查询时间 超过1秒则为慢查询
binlog_checksum=NONE               #默认为NONE， 表示在图1的箭头1 不生成checksum， 这样就可以兼容旧版本的mysql。此外，就只能设置为CRC32了。
binlog_format=MIXED                #设定主从复制模式的方法非常简单，只要在以前设定复制配置的基础上，再加一个参数：以上两种模式的混合使用，一般的复制使用STATEMENT模式保存binlog，对于STATEMENT模式无法复制的操作使用ROW模式保存binlog，MySQL会根据执行的SQL语句选择日志保存方式。
log-bin                 = /opt/mysql/mysql-bin.log    # binlog日志文件
expire_logs_days        = 7                           # binlog过期清理时间
max_binlog_size         = 100m                        # binlog每个日志文件大小
binlog_cache_size       = 4m                          # binlog缓存大小
max_binlog_cache_size   = 512m                        # 最大binlog缓存大小

symbolic-links=0
log_bin=mysql-bin
character_set_server=utf8                            #写入的数据到MySQL中数据为防止中文乱码。
lower_case_table_names=1                             #这样MySQL 将在创建与查找时将所有的表名自动转换为小写字符
character_set_client=utf8                            #集群写入的数据到MySQL中数据为防止中文乱码。
collation-server=utf8_general_ci

sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

[mysqld_safe]
log-error=/opt/mysql/log/error.log   #错误日志路径
log=/opt/mysql/log/mysql.log         #普通日志路径
master-port=3306                     #mysql master端口
log-slave-updates

[mysql]

[client]
default-character-set=utf8
socket=/var/lib/mysql/mysql.sock
```
MySQL配置my.cnf 优化报错

** Mysql参数优化对于新手来讲，是比较难懂的东西，其实这个参数优化，是个很复杂的东西，对于不同的网站，及其在线量，访问量，帖子数量，网络情况，以及机器硬件配置都有关系，优化不可能一次性完成，需要不断的观察以及调试，才有可能得到最佳效果。**

个人在配置这个时候碰到很多的问题：

比如出现配置完。启动报错：
```
Starting MySQL...The server quit without updating PID file (/opt/mysql/data/rekfan.pid).[失败]
可参考这篇：http://blog.rekfan.com/articles/186.html
我的问题解决是把自己配置定义的pid 
pid-file=/opt/mysql/log/mysql.pid
注释掉，让默认去访问。
因为启动/etc/init.d/mysql 下面的脚本 rpm安装的默认路径在/var/lib/mysql/mysql-2.pid

还有报错：

报出异常Can 't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock '(2) "
打开错误日志/opt/mysql/log/error.log看到如下内容：
140417  7:30:38 [ERROR] Can't start server: Bind on TCP/IP port: Cannot assign requested address  
140417  7:30:38 [ERROR] Do you already have another mysqld server running on port: 3306 ?g  
140417  7:30:38 [ERROR] Aborting  
端口占用
kill进程重新启动。
添加了这条：
socket=/var/lib/mysql/mysql.sock


修改配置文件：先cp

cp /usr/share/mysql/my-default.cnf /etc/my.cnf
然后配置/etc/my.cnf
```
