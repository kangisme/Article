**本文假设ip地址为111.111.111.111,用户名为ftpuser，密码为password，请实际操作时套用自己的设定**

### 1.安装并启动 FTP 服务 ###
**安装 VSFTPD**  
使用 yum 安装 vsftpd：  
yum install vsftpd -y

**启动 VSFTPD**  
安装完成后，启动 FTP 服务：  
service vsftpd start  
启动后，可以看到系统已经监听了 21 端口：  
netstat -nltp | grep 21
此时，访问 ftp://111.111.111.111 可浏览机器上的 /var/ftp 目录了。

### 2.配置 FTP 权限 ###
**了解 VSFTP 配置**  
vsftpd 的配置目录为 /etc/vsftpd，包含下列的配置文件：  
vsftpd.conf 为主要配置文件  
ftpusers 配置禁止访问 FTP 服务器的用户列表  
user_list 配置用户访问控制  

**阻止匿名访问和切换根目录**  
匿名访问和切换根目录都会给服务器带来安全风险，我们把这两个功能关闭。  
编辑 /etc/vsftpd/vsftpd.conf，找到下面两处配置
并修改：  
\# 禁用匿名用户  
anonymous_enable=NO

\# 禁止切换根目录  
chroot_local_user=YES
编辑完成后，按Esc退出编辑，按两次Shift+Z保存配置即可，重新启动 FTP 服务，如：
systemctl restart vsftpd  

### 3.创建 FTP 用户 ###  
创建一个用户 ftpuser：  
useradd ftpuser  
为用户 ftpuser 设置密码：  
echo "password" | passwd ftpuser --stdin
 
**限制该用户仅能通过 FTP 访问**  
限制用户 ftpuser 只能通过 FTP 访问服务器，而不能直接登录服务器：  
usermod -s /sbin/nologin ftpuser
  
**为用户分配主目录**  
为用户 ftpuser 创建主目录，并约定：  
/data/ftp 为主目录, 该目录不可上传文件  
/data/ftp/pub 文件只能上传到该目录下

mkdir -p /data/ftp/pub  
创建登录欢迎文件：  
echo "Welcome to use FTP service." > /data/ftp/welcome.txt  
设置访问权限：  
chmod a-w /data/ftp && chmod 777 -R /data/ftp/pub  
设置为用户的主目录：  
usermod -d /data/ftp ftpuser  

### 4.访问 FTP 服务 ###  
FTP 服务已安装并配置完成，下面我们来使用该 FTP 服务    
根据你个人的工作环境，选择一种方式来访问已经搭建的 FTP 服务
 
**通过 Windows 资源管理器访问**  
Windows 用户可以通过复制下面的链接到资源管理器的地址栏访问：  
ftp://ftpuser:password@111.111.111.111

**通过 FTP 客户端工具访问**  
FTP 客户端工具众多，下面推荐两个常用的：  
WinSCP - Windows 下的 FTP 和 SFTP 连接客户端  
FileZilla - 跨平台的 FTP 客户端，支持 Windows 和 Mac

下载和安装 FTP 客户端后，使用下面的凭据进行连接即可：  
主机：  
111.111.111.111  
用户：  
ftpuser  
密码：  
password  
如果能够正常连接，那就大功告成了。