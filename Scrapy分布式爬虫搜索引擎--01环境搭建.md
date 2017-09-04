# **Scrapy分布式爬虫搜索引擎**
## **1. 前言**
这段时间将会学习scrapy爬虫框架和基于此的分布式爬虫，并试着结合django框架和elasticsearch搭建搜索引擎网站。    
计划的学习步骤是：
1. 环境配置和相关基础知识学习
2. 爬取有效数据
3. scrapy反爬虫突破
4. scrapy进阶
5. scrapy redis 分布式爬虫
6. elasticsearch django 实现搜索引擎网站  

希望能成功吧... 
## **2. 环境搭建**
开发环境如下：
1. 数据库-----mysql,redis,elasticsearch
2. 开发环境-----anconda
3. django==1.11.3
4. scrapy==1.3.3  

这次编写并没有使用IDE环境(pycharm)，而是在win平台上配置vscode来进行代码编写。再将代码部署到Linux。  
具体的环境搭建随着学习步骤的进行再继续讲解，在这里只讲数据库mysql和使用anconda安装python开发环境(python2.7和python3.6)。

## **2.1 mysql安装**
### **2.1.1 windows下安装**
直接从[mysql官网下载](https://dev.mysql.com/downloads/installer/)下载安装即可。  
国内也可以从[清华镜像站](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)下载。
### **2.1.2 Linux下安装**
Linux使用的是CentOS作为服务器，安装mysql命令如下：
>yum install mysql-server -y  

安装完成后启动mysql服务:
>service mysqld restart

设置mysqlz账户root密码：
>/usr/bin/mysqladmin -u root password 'Password'

将mysql设置为开机启动：
>chkconfig mysqld on

另外因为需要远程访问mysql数据库，因此还需要对mysql进行相关的权限配置。默认情况下mysqlzhi只允许本地用户访问。外网IP用户如果需要远程到mysql需要对`mysql数据库`里的'*user*'表里添加相关授权。

例如:需要让用户wujiu使用newpwd密码从IP:192.169.1.3主机连接到服务器

具体步骤：  

>mysql>GRANT ALL PRIVILEGES ON . TO 'wujiu'@'192.169.1.3' IDENTIFIED BY 'newpwd' WITH GRANT OPTION;  

然后刷新一下权限：
>mysql>flush privileges;


grant语法:
>grant 权限名(所有权限用all) on 库名.表名(全部用 `.` ) to '要授权的用户'@'IP( `%` 表示所有的IP)' identified by '密码' with grant option;

这样就能远程访问到相应的数据库了。
## **2.2 anconda配置** 
### **2.2.1 anconda是什么**
anconda是一款可用于科学计算的python发行版,支持Linux，Mac，Windows,内置了常用的科学计算包。这里是用它是因为它解决了官方pythond的两大痛点：  
>1. 提供了包管理功能, Windows平台安装第三方包经常失败的轻快的得以解决;  
>2. 提供环境管理的功能,功能类似于virtualenv,解决了python多版本并存、切换的问题；

### **2.2.2 安装anconda**
直接从[anconda官网](https://www.anaconda.com/download/)下载相应版本默认安装即可。

特别需要注意的是linux可以使用wget下载：

for python2.7:
>`wget http://repo.continuum.io/archive/Anaconda2-4.3.0-Linux-x86_64.sh`

for python3:
>`wget http://repo.continuum.io/archive/Anaconda3-4.3.0-Linux-x86_64.sh` 

安装anconda:
>`bash Anaconda3-4.3.0-Linux-x86_64.sh`

### **2.2.3 conda工具介绍**
conda是anconda下用于包管理和环境管理的工具,功能上类似于pip和virtualenv的组合。安装成功后conda会默认加入到环境变量中,因此可以直接在命令行窗口运行命令**conda**。

conda 的环境管理与virtualenv基本上是类似的操作。在安装后会默认安装一个名为root的python环境。

    # 查看帮助
    conda -h
    # 基于python3.6创建一个名为python36的环境
    conda create -n python36 python=3.6
    # 基于python2.7创建一个名为python27的环境
    conda create -n python27 python=2.7 
    # 激活环境
    activate python36
    source activate python27 #mac/linux
    # 检查python版本，这里显示的应是3.6
    python -V
    # 退出当前环境
    deactivate python36
    # 删除当前环境
    conda remove -n python36 --all
    # or
    conda env remove -n python36
    
    # 查看所有安装的环境
    conda env list # 前面有*的是当前环境
    # or
    conda info -e 
conda包管理功能和pip一样,也可以使用pip来进行安装。

    # 安装 matplotlib
    conda install matplotlib
    # 查看已安装的包
    conda list
    # 包更新
    conda update matplotlib
    # 包删除
    conda remove matplotlib
    # 更新 conda
    conda update conda
    # 更新 anconda
    conda update anconda
    # 更新python
    conda update python
### **2.2.4 更改国内源**
anconda默认的镜像地址在国外，访问速度慢，在国内目前可用的镜像地址是清华镜像站。修改方式如下：

     # 在命令行输入
     conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
     conda config --set show_channel_urls yes    
### **2.2.5 安装django和scrapy**
在python36和python27环境使用conda命令安装django和scrapy:
>`conda install django`  
>`conda install scrapy`

    


  