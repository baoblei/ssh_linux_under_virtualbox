远程连接宿主机上的虚拟机方法
# 准备工作
-   宿主机器通过virtualBox安装linux虚拟机
-   宿主机可以访问公网
-   虚拟机上安装ssh服务
查看是否安装：`rpm -qa|grep ssh`
```bash
sudo apt-get install openssh-server
```
这个命令将会安装OpenSSH服务器。完成后，你就可以启动SSH服务并连接到Linux服务器了。
-  配置ssh服务器
在安装完SSH服务器之后，你需要对其进行一些配置以确保安全运行。通过编辑SSH配置文件可以完成这个任务。
```bash
sudo vim /etc/ssh/sshd_config
```
通过编辑该文件，你可以更改SSH服务器的端口、密钥等配置项。为了提高安全性，建议将SSH服务器的默认端口号22更改为其他端口号。
允许root用户远程登录: 找到PermitRootLogin，将参数prohibit-password改为yes，原来是prohibit-password
另外，你可以将密钥验证方式从密码验证方式更改为公钥身份验证方式。公钥验证能够更好地防止黑客攻击。
-   ssh开启，关闭和重启
```bash
service sshd start
service sshd stop
service sshd restart
```
修改配置文件后需要重启ssh服务
-   Linux 用户管理 
参考：https://www.runoob.com/linux/linux-user-manage.html
    -   添加用户`useradd 选项 用户名`
        选项 -d  可以指定用户主目录/home/用户名
    -   删除用户`userdel 选项 用户名`
        选项 -r 把用户主目录一起删除
    -   用户密码管理 `passwd 用户名`

# 有路由器管理权限
-   路由器管理系统中设置内外网端口映射
-   最好给宿主机分配一个静态的私有地址，防止路由器重启宿主机ip地址发生变化
-   找到路由器的公网ip（WAN口地址），远程主机可以ping一下试试是否可以建立连接
-   virtualbox中设置网络
    -   NAT 高级设置中编辑端口转发规则
        宿主端口自定义，子系统端口为默认为22
-   此时远程主机就可以通过`ssh usr@ip -p port'连接宿主电脑的虚拟机了
    -   ip 为公网地址
    -   port 为用户设置的端口  
-   远程电脑可以修改/etc/ssh/ssh_config  文件给ssh服务器设置别名。格式为：
```
Host 别名
    HostName  ssh地址
    User 用户名
    Port 端口
```
# 无路由器管理权限
借助内网穿透工具如花生壳实现外网访问