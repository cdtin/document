# CentOS7 安装 Shadowsocks(仅适用于KVM架构)

## 查看系统架构
```shell
sudo yum install virt-what
sudo virt-what
```

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
    "timeout": 600
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

4. 查看Shadowsocks服务是否启动成功
```shell
systemctl status shadowsocks -l
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
sudo grub2-set-default 0
```

## 重启VPS
```shell
sudo reboot
```

## 查看系统内核版本(需要显示4.1以上)
```shell
sudo uname -r
```

## 开启BBR
1. 打开sysctl.conf文件
```shell
sudo vi /etc/sysctl.conf 
```
2. 在文件末尾添加如下配置
```config
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr
```

## 查看配置是否添加成功
```shell
sudo sysctl -p
```
- 如果显示有刚添加的两条数据表示成功

## 查看BBR是否成功开启
```shell
sudo sysctl net.ipv4.tcp_available_congestion_control
sudo lsmod | grep bbr
```

# THE END

#### 相关客户端

- [Windows](https://github.com/shadowsocks/shadowsocks-windows/releases)
 
- [Mac](https://github.com/shadowsocks/ShadowsocksX-NG/releases)

- [Android](https://github.com/shadowsocks/shadowsocks-android/releases)