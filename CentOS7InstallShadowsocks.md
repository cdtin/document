# CentOS7 安装 Shadowsocks

## 修改登录密码
```shell
    sudo passwd
```

## 安装pip包管理工具
``` shell
    sudo curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
    sudo python get-pip.py
    sudo pip install --upgrade pip
```

## 安装配置Shadowsocks
``` shell
    sudo pip install shadowsocks
```

1. 创建Shadowsocks配置文件
``` shell
    sudo vi /etc/shadowsocks.json
```
2. 加入如下配置到shadowsocks.json文件中
``` json
    {
        "server": "0.0.0.0",
        "server_port": 443,
        "password": "ABC123qwe",
        "method": "aes-256-cfb",
        "timeout": 600,
    }
```

## 配置Shadowsocks开机自启动

1.  打开shadowsocks.service文件

```shell
    sudo vi /etc/systemd/system/shadowsocks.service
```
2.加入下配置到shadowsocks.service文件中

``` config
[Unit]
Description=Shadowsocks

[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/ssserver -c /etc/shadowsocks.json

[Install]
WantedBy=multi-user.target
```

3. 启动Shadowsocks服务
``` shell
    sudo systemctl enable shadowsocks
    sudo systemctl start shadowsocks
```

## 开启端口号
```shell
    sudo firewall-cmd --zone=public --add-port=443/tcp --permanent
    sudo firewall-cmd --reload
```

---


# 配置BBR加速

## 更新系统到最新版本
```shell
    sudo yum update
```

## 安装elrepo并升级内核
```shell
    sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
    sudo rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
    sudo yum --enablerepo=elrepo-kernel install kernel-ml -y
```

## 更新grub文件
```shell
    sudo egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \'
```