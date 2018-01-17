---
title: Teradata prel脚本连接ODBC
layout: post
categories: ''
tags: ''
---
如下代码所示：
    用最简单的例子，明白关键代码，prel脚本如下

        use DBI;

	$userid='dbc';
	$passwd='dbc';
	$dsn=192.168.15.141;

	$dbh=DBI->connect("dbi:ODBC:$dsn",$userid,$passwd);  

报错：
    
    TD-EXPRESS:~ # perl test.pl 
    install_driver(ODBC) failed: Can't locate DBD/ODBC.pm in @INC (@INC contains: /usr/lib64 /usr/lib/perl5/5.10.0/x86_64-linux-thread-multi /usr/lib/perl5/5.10.0 /usr/lib/perl    5/site_perl/5.10.0/x86_64-linux-thread-multi /usr/lib/perl5/site_perl/5.10.0 /usr/lib/perl5/vendor_perl/5.10.0/x86_64-linux-thread-multi /usr/lib/perl5/vendor_perl/5.10.0 /    usr/lib/perl5/vendor_perl .) at (eval 3) line 3.
    Perhaps the DBD::ODBC perl module hasn't been fully installed,
    or perhaps the capitalisation of 'ODBC' isn't right.
    Available drivers: DBM, ExampleP, File, Gofer, Proxy, SQLite, Sponge.
     at test.pl line 7	
     
DBI是一个用于Perl编程语言的标准数据库独立接口模块。它定义了一组方法、变量和约定，它们提供了与实际使用的数据库无关的一致的数据库接口。DBI是基于模块架构构建的——对于每个数据库，都有一个驱动程序，它实现了特定数据库的工作细节。DBI允许Perl应用程序使用特定的数据库驱动程序-DBD模块连接到数据库。DBD模块处理与各种数据库交互的复杂细节  

DBD::ODBC是DBI的ODBC驱动程序。它用于连接基于ODBC的数据源。为了验证DBI和DBD::ODBC模块被正确安装和操作，执行以下脚本行:    


    perl -MDBI -e "DBI->installed_versions"  
    
The response should include DBD::ODBC module and its version similar to the following example:  
![TIM截图20180116112658](http://p1vuoao0b.bkt.clouddn.com/JekyllWriter/TIM截图20180116112658.png)

##Prerequisites
下载：[DBD::ODBC](http://search.cpan.org/~mjevans/DBD-ODBC-1.52/ODBC.pm)

由于Teradata ODBC驱动程序是使用DataDirect驱动程序管理器分配的，因此需要使用一个与DataDirect驱动程序管理器构建的数据库来取代DBD::ODBC模块。

在完成安装并配置后，报错：  
![TIM截图20180116222849](http://p1vuoao0b.bkt.clouddn.com/JekyllWriter/TIM截图20180116222849.png)  
看到的反应肯定是百度安装[DBI](http://search.cpan.org/~timb/DBI-1.634/Changes)了，首先下载[DBI](http://search.cpan.org/~timb/DBI-1.634/Changes)  
解压dbi:  ```tar -zxvf DIB压缩包.tar.gz```  
然后进入DBI目录，执行：    

        perl Makefile.PL
        make 
        make test
        make install
        
再次执行脚本,报错： 
 
        perl: symbol lookup error: /usr/lib/perl5/site_perl/5.10.0/x86_64-linux-thread-multi/auto/DBD/ODBC/ODBC.so: undefined symbol: SQLAllocHandle  
        
经过多方搜索终于找到解决办法： 添加*__$opts{LIBS} = "-L$odbclibdir -lodbc;__*在如下代码中

        ...
    elsif ($myodbc eq 'intersolve') {
    $opts{DEFINE} = "";
    #print {$sqlhfh} qq{#include <qeodbc.h>\n};
    if (-f "$odbchome/include/sql.h") {
    print "You seem to have the official header files set for DataDirect.\n";
    $opts{INC} .= " -I$odbchome/include";
    $opts{LIBS} = "-L$odbclibdir -lodbc;
    print {$sqlhfh} qq{#include <sql.h>\n#include <sqltypes.h>\n#include <sqlext.h>\n#include <sqlucode.h>\n};
    }
。
。