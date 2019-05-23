---
title: vsftpd安装与配置
date: 2019-05-22 16:18:06
categories: 技术
tags: [linux,vsftpd]
---
vsftpd 是“very secure FTP daemon”的缩写，安全性是它的一个最大的特点。vsftpd 是一个 UNIX 类操作系统上运行的服务器的名字，它可以运行在诸如 Linux、BSD、Solaris、 HP-UNIX等系统上面，是一个完全免费的、开放源代码的ftp服务器软件，支持很多其他的 FTP 服务器所不支持的特征。比如：非常高的安全性需求、带宽限制、良好的可伸缩性、可创建虚拟用户、支持IPv6、速率高等。
<!-- more --> 
## 1. 安装vsftpd
对于ubuntu系统可以很容易的通过apt-get来安装vsftpd
```
sudo apt-get install vsftpd
```
其配置文件在
```
/etc/vsftpd.conf
```
根据实际环境需要修改配置文件：
```
#log_ftp_protocol=YES
listen=YES
listen_port=3001 # ftp登陆端口
ftp_data_port=3000 # 主动模式数据端口
port_enable=YES
port_promiscuous=YES
pasv_enable=YES
pasv_address=  # 此处填被动模式所需的外网IP
pasv_addr_resolve=yes
pasv_promiscuous=YES
delete_failed_uploads=YES
#listen_ipv6=YES
# Allow anonymous FTP? (Disabled by default)
anonymous_enable=NO
# Uncomment this to allow local users to log in.
local_enable=YES
# Uncomment this to enable any form of FTP write command.
write_enable=YES
# Default umask for local users is 077. You may wish to change this to 022,
# if your users expect that (022 is used by most other ftpd's)
local_umask=022
#file_open_mode=0755
#pasv_enable=NO
# Uncomment this to allow the anonymous FTP user to upload files. This only
# has an effect if the above global write enable is activated. Also, you will
# obviously need to create a directory writable by the FTP user.
anon_upload_enable=NO
# Uncomment this if you want the anonymous FTP user to be able to create
# new directories.
anon_mkdir_write_enable=NO
# Activate directory messages - messages given to remote users when they
# go into a certain directory.
dirmessage_enable=YES
# If enabled, vsftpd will display directory listings with the time
# in  your  local  time  zone.  The default is to display GMT. The
# times returned by the MDTM FTP command are also affected by this
# option.
#use_localtime=YES
syslog_enable=YES
# Activate logging of uploads/downloads.
xferlog_enable=YES
# Make sure PORT transfer connections originate from port 20 (ftp-data).
connect_from_port_20=YES
# If you want, you can arrange for uploaded anonymous files to be owned by
# a different user. Note! Using "root" for uploaded files is not
# recommended!
#chown_uploads=YES
#chown_username=whoever
# You may override where the log file goes if you like. The default is shown
# below.
xferlog_file=/var/log/xferlog.log
# If you want, you can have your log file in standard ftpd xferlog format.
# Note that the default log file location is /var/log/xferlog in this case.
xferlog_std_format=YES
dual_log_enable=YES
vsftpd_log_file=/var/log/vsftpd.log
# You may change the default value for timing out an idle session.
#idle_session_timeout=600
# You may change the default value for timing out a data connection.
#data_connection_timeout=120  # 数据连接超时
# It is recommended that you define on your system a unique user which the
# ftp server can use as a totally isolated and unprivileged user.
#nopriv_user=ftpsecure
# Enable this and the server will recognise asynchronous ABOR requests. Not
# recommended for security (the code is non-trivial). Not enabling it,
# however, may confuse older FTP clients.
#async_abor_enable=YES
# By default the server will pretend to allow ASCII mode but in fact ignore
# the request. Turn on the below options to have the server actually do ASCII
# mangling on files when in ASCII mode.
# Beware that on some FTP servers, ASCII support allows a denial of service
# attack (DoS) via the command "SIZE /big/file" in ASCII mode. vsftpd
# predicted this attack and has always been safe, reporting the size of the
# raw file.
# ASCII mangling is a horrible feature of the protocol.
ascii_upload_enable=YES
ascii_download_enable=YES
# You may fully customise the login banner string:
#ftpd_banner=Welcome to blah FTP service.
# You may specify a file of disallowed anonymous e-mail addresses. Apparently
# useful for combatting certain DoS attacks.
#deny_email_enable=YES
# (default follows)
#banned_email_file=/etc/vsftpd.banned_emails
# You may restrict local users to their home directories.  See the FAQ for
# the possible risks in this before using chroot_local_user or
# chroot_list_enable below.
#chroot_local_user=YES
# You may specify an explicit list of local users to chroot() to their home
# directory. If chroot_local_user is YES, then this list becomes a list of
# users to NOT chroot().
# (Warning! chroot'ing can be very dangerous. If using chroot, make sure that
# the user does not have write access to the top level directory within the
# chroot)
chroot_local_user=YES
allow_writeable_chroot=YES
chroot_list_enable=YES 
# (default follows)
chroot_list_file=/etc/vsftpd.chroot_list 
#chroot_list_enable=YES chroot_local_user=YES
#1.所有用户都被限制在其主目录下 2.使用chroot_list_file指定的用户列表，这些用户作为“例外”，不受限制
#chroot_list_enable=YES chroot_local_user=NO
#1.所有用户都不被限制其主目录下 2.使用chroot_list_file指定的用户列表，这些用户作为“例外”，受到限制
#chroot_list_enable=NO chroot_local_user=YES
#1.所有用户都被限制在其主目录下 2.不使用chroot_list_file指定的用户列表，没有任何“例外”用户
#chroot_list_enable=NO chroot_local_user=NO
#1.所有用户都不被限制其主目录下 2.不使用chroot_list_file指定的用户列表，没有任何“例外”用户
# You may activate the "-R" option to the builtin ls. This is disabled by
# default to avoid remote users being able to cause excessive I/O on large
# sites. However, some broken FTP clients such as "ncftp" and "mirror" assume
# the presence of the "-R" option, so there is a strong case for enabling it.
#ls_recurse_enable=YES
# This option should be the name of a directory which is empty.  Also, the
# directory should not be writable by the ftp user. This directory is used
# as a secure chroot() jail at times vsftpd does not require filesystem
# access.
secure_chroot_dir=/var/run/vsftpd/empty
# This string is the name of the PAM service vsftpd will use.
pam_service_name=vsftpd
# This option specifies the location of the RSA certificate to use for SSL
# encrypted connections.
#rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
# This option specifies the location of the RSA key to use for SSL
# encrypted connections.
#local_root=/home/uftp
#rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
userlist_deny=NO
userlist_enable=YES
userlist_file=/etc/allowed_users 
# 当且仅当userlist_enable=YES时：userlist_deny项的配置才有效，user_list文件才会被使用；当其为NO时，无论userlist_deny项为何值都是无效的，本地全体用户（除去ftpusers中的用户）都可以登入FTP
# 当userlist_enable=YES时，userlist_deny=YES时：user_list是一个黑名单，即：所有出现在名单中的用户都会被拒绝登入；
# 当userlist_enable=YES时，userlist_deny=NO时：user_list是一个白名单，即：只有出现在名单中的用户才会被准许登入(user_list之外的用户都被拒绝登入)；另外需要特别提醒的是：使用白名单后，匿名用户将无法登入！除非显式在user_list中加入一行：anonymous
seccomp_sandbox=NO
# ssl config
ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=NO
force_local_logins_ssl=NO
force_anon_logins_ssl=NO
force_anon_data_ssl=NO
ssl_tlsv1=YES
ssl_sslv2=YES
ssl_sslv3=YES
require_ssl_reuse=NO
debug_ssl=YES
strict_ssl_read_eof=YES
strict_ssl_write_shutdown=YES
ssl_ciphers=HIGH
rsa_cert_file=/etc/ssl/certs/vsftpd.pem
rsa_private_key_file=/etc/ssl/certs/vsftpd.pem
pasv_max_port=31000
pasv_min_port=30000
data_connection_timeout=8
```
/etc/vsftpd.chroot_list 中加入root用户作为例外，可以不被限制在主目录下
/etc/allow_users 中加入允许登陆的用户
## 2. 创建用户
```
useradd -d /home/temp temp
passwd temp
usermod -s /usr/sbin/nologin temp
```
添加用户temp，用户根目录/home/temp，设置密码，设置权限为/usr/sbin/nologin,禁止登陆，此处需要注意不同系统nologin的目录不同。

