# CentOS搭建SVN

## 安装说明

- 系统环境：CentOS-6.0以上
- 安装方式：yum install （源码安装容易产生版本兼容的问题）
 
## 检查已安装版本

- 检查是否安装了低版本的SVN

    > rpm -qa subversion

- 如果存在，卸载掉旧版本SVN

    > yum remove subversion  

## 安装SVN

    > yum install httpd httpd-devel subversion mod_dav_svn mod_auth_mysql

- 确认已安装了svn模块

    > cd /etc/httpd/modules  
    > ls | grep svn  

出现以下内容，表示安装svn模块安装成功
```
    mod_authz_svn.so
    mod_dav_svn.so
```

- 验证安装，检验已经安装的SVN版本信息 

    > svnserve --version
    
svnserve，版本 1.6.11 (r934486)
编译于 Jun 23 2012，00:44:03
版权所有 (C) 2000-2009 CollabNet。 
Subversion 是开放源代码软件，请参阅 http://subversion.tigris.org/ 站点。 
此产品包含由 CollabNet(http://www.Collab.Net/) 开发的软件。
下列版本库后端(FS) 模块可用:
* fs_base : 模块只能操作BDB版本库。 
* fs_fs : 模块与文本文件(FSFS)版本库一起工作。
Cyrus SASL 认证可用。
代码库创建
SVN软件安装完成后还需要建立SVN库 
[root@localhost modules]# mkdir -p /opt/svn/repositories
[root@localhost modules]# svnadmin create /opt/svn/repositories
执行上面的命令后，自动建立repositories库，查看/opt/svn/repositories 文件夹发现包含了conf, db,format,hooks, locks, README.txt等文件，说明一个SVN库已经建立。
配置代码库
进入上面生成的文件夹conf下，进行配置 
[root@localhost modules]# cd /opt/svn/repositories/conf
用户密码passwd配置
[root@localhost password]# cd /opt/svn/repositories/conf
[root@admin conf]# vi + passwd
修改passwd为以下内容：
[users]
# harry = harryssecret
# sally = sallyssecret
zhoulf=123456
权限控制authz配置
[root@admin conf]# vi + authz
目的是设置哪些用户可以访问哪些目录，向authz文件追加以下内容：
#设置[/]代表根目录下所有的资源 
[/]
zhoulf=rw
服务svnserve.conf配置
[root@admin conf]# vi + svnserve.conf
追加以下内容：
[general]
#匿名访问的权限，可以是read,write,none,默认为read
anon-access=none
#使授权用户有写权限 
auth-access=write
#密码数据库的路径 
password-db=passwd
#访问控制文件 
authz-db=authz
#认证命名空间，subversion会在认证提示里显示，并且作为凭证缓存的关键字 
realm=/opt/svn/repositories
配置防火墙端口
[root@localhost conf]# vi /etc/sysconfig/iptables
添加以下内容： 
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3690 -j ACCEPT
保存后重启防火墙 
[root@localhost conf]# service iptables restart
启动SVN
svnserve -d -r /opt/svn/repositories
查看SVN进程
[root@localhost conf]# ps -ef|grep svn|grep -v grep
root     12538     1  0 14:40 ?        00:00:00 svnserve -d -r /opt/svn/repositories
检测SVN 端口
[root@localhost conf]# netstat -ln |grep 3690
tcp        0      0 0.0.0.0:3690                0.0.0.0:*                   LISTEN
停止重启SVN
[root@localhost password]# killall svnserve    //停止 
[root@localhost password]# svnserve -d -r /opt/svn/repositories  // 启动
测试
SVN服务已经启动，使用客户端测试连接。 
客户端连接地址：svn://192.168.15.231
用户名/密码： zhoulf/123456
测试创建文件夹等操作。

