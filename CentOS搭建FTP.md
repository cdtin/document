# CentOS搭建FTP服务器

### 安装vsftpd

```shell
yum -y install vsftpd
```

### 配置vsftpd.conf文件

```shell
vim /etc/vsftpd/vsftpd.conf
```
> 在vsftpd.conf文件中配置如下信息  

```vsftpd.conf
local_enable=YES

write_enable=YES

local_umask=022

dirmessage_enable=YES

xferlog_enable=YES

connect_from_port_20=YES

xferlog_std_format=YES

# 设置连接超时时间（单位秒）
idle_session_timeout=600

# 数据传输超时时间（单位秒）
data_connection_timeout=120

# 是否允许上传/下载二进制文件
ascii_upload_enable=YES
ascii_download_enable=YES

# 限制所有的本地用户只能访问自己的目录
chroot_local_user=YES
 
# 是否允许使用ls -R等命令
ls_recurse_enable=YES

listen=YES

pam_service_name=vsftpd
userlist_enable=YES

# YES，默认值，禁止文件中的用户登录，同时也不向这些用户发出输入密码的提示。NO，只允许在文件中的用户登录FTP服务器
userlist_deny=NO 
 
tcp_wrappers=YES
 
# [新增]所有用户的根目录（对匿名用户无效）
local_root=/yddata
```


### 新增ftp用户并为其设置密码

```shell
useradd [ftpLoginName] -s /sbin/nologin 
passwd [ftpLoginName]   #会提示要求设置密码，两次输入必须一致

#[ftpLoginName]表示设置ftp登陆的用户名，设置的时候不需要[]
```
### 编辑user_list文件，给ftpLoginName用户分配ftp访问权限

```shell
vim /etc/vsftpd/user_list
```
> 在user_list文件末尾加入ftpLoginName并保存

### 创建ftp目录并指定用户

```shell
#在指定目录创建ftp文件夹
mkdir -p /ftpFile
#指定ftp可访问的用户
chown -R ftpLoginName /ftpFile/   #注意后面有个/
#设置ftp文件夹权限
chmod -R 755 /ftpFile
```

### 启动vsftpd服务
```shell
service vsftpd start
```

### 开机启动vsftpd服务

```shell
chkconfig vsftpd on
```

# END

