# CentOS搭建SVN

## 安装说明

- 系统环境：CentOS-6.0以上
- 安装方式：yum install （源码安装容易产生版本兼容的问题）
 
## 检查已安装版本

- 检查是否安装了低版本的SVN

```shell
    rpm -qa subversion
```
- 如果存在，卸载掉旧版本SVN

```shell
     yum remove subversion  
```
## 安装SVN

```shell
     yum install httpd httpd-devel subversion mod_dav_svn mod_auth_mysql
```
- 确认已安装了svn模块

```shell
     cd /etc/httpd/modules  
     ls | grep svn  
```
出现以下内容，表示安装svn模块安装成功

> mod_authz_svn.so  
> mod_dav_svn.so


- 验证安装，检验已经安装的SVN版本信息 

```shell
     svnserve --version
```
出现下面内容表示svn安装成功
  
> svnserve，版本 1.6.11 (r934486)  
> 编译于 Jun 23 2012，00:44:03
    版权所有 (C) 2000-2009 CollabNet。    
> Subversion 是开放源代码软件，请参阅 http://subversion.tigris.org/ 站点。 



- 代码库创建

SVN软件安装完成后还需要建立SVN库

```shell
    mkdir -p /opt/svn/repositories  
    svnadmin create /opt/svn/repositories
```

> 执行上面的命令后，自动建立repositories库，查看/opt/svn/repositories 文件夹发现包含了conf, db,format,hooks, locks, README.txt等文件，说明一个SVN库已经建立。

- 配置代码库

     进入上面生成的文件夹conf下，进行配置 

```shell
     cd /opt/svn/repositories/conf
```

- 用户密码passwd配置

```shell
    cd /opt/svn/repositories/conf
    vi + passwd
```

修改passwd为以下内容：

```shell
    [users]
    tin=123456
    webcode=123456
```

- 用户权限控制authz配置

```shell
    vi + authz
```
> 目的是设置哪些用户可以访问哪些目录，向authz文件追加以下内容：  
> 设置[/]代表根目录下所有的资源， [/WebCode]只能读写WebCode下的资源

```shell
    [/]
    tin=rw 

    [/WebCode]
    webcode=rw;
```

- 服务svnserve.conf配置

```shell
    vi + svnserve.conf
```
追加以下内容：

```shell
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
```


-   配置防火墙端口

```shell
    vi /etc/sysconfig/iptables
```

添加以下内容： 

```shell
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 3690 -j ACCEPT
```

保存后重启防火墙 

```shell
    service iptables restart
```

启动SVN

```shell
svnserve -d -r /opt/svn/repositories
```

查看SVN进程

```shell
    ps -ef|grep svn|grep -v grep
```
> 获得如下内容:  
root     12538     1  0 14:40 ?        00:00:00 svnserve -d -r /opt/svn/repositories


检测SVN 端口

```shell
    netstat -ln |grep 3690
```

端口监听成功：

> tcp        0      0 0.0.0.0:3690                0.0.0.0:*                   LISTEN

- 停止重启SVN


```shell
    killall svnserve    //停止 
```

```shell
    svnserve -d -r /opt/svn/repositories  // 启动
```

- 测试

    > SVN服务已经启动成功，使用客户端测试连接 [TortoiseSVN](https://tortoisesvn.net/downloads.zh.html)


    > 客户端连接地址：svn://104.236.166.246  
    > 用户名/密码： tin/123456

# END

