---
title: Windows下 maven的安装步骤+maven配置本地仓库
layout: post
---
1. 下载[maven](http://maven.apache.org/download.cgi)
2. 把apache-maven-3.5.2-bin.zip解压目录D:\MyApplication\maven\apache-maven-3.5.2
3. 配置maven的环境变量：先配置M2_HOME的环境变量，新建一个系统变量：M2_HOME , 路径是：D:\MyApplication\maven\apache-maven-3.5.2，如图所示：  
![TIM截图20180118153529](http://p1vuoao0b.bkt.clouddn.com/JekyllWriter/TIM截图20180118153529.png)  
4. 再配置path环境变量，在path值的末尾添加"%M2_HOME%\bin"，如下图所示：

![TIM截图20180118153513](http://p1vuoao0b.bkt.clouddn.com/JekyllWriter/TIM截图20180118153513.png)  
5. 点击确定之后，打开cmd窗口：输入 mvn -version,出现如下内容表示安装成功  
![TIM截图20180118154124](http://p1vuoao0b.bkt.clouddn.com/JekyllWriter/TIM截图20180118154124.png)