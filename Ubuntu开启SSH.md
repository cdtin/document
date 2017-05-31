判断是否安装ssh服务：
Ps –e|grep ssh

安装ssh-client：
Sudo apt-get install openssh-client
安装ssh-server:
Sudo apt-get install openssh-server
启动服务：
Sudo /etc/init.d/ssh start

Ssh服务默认的端口是22，可以修改
Sudo gedit /etc/ssh/sshd_config  --gedit命令只能在可视化界面使用
修改后重启
Sudo /etc/init.d/ssh restart

