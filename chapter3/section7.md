# 实战php程序自动发布

4.1 插件安装

系统管理—管理插件—已安装

检查是否有“Git plugin”和“Publish Over SSH”两个插件，如果没有，则需点击“可选插件”，找到它并安装

安装好两个插件后，点击“系统管理”– “系统设置”

4.2 SSH配置

系统管理—系统设置—Publish over SSH

在key内填写jenkins服务器的私钥，如果没有需要先在jenkins服务器生成私钥与公钥。ssh-keygen -t rsa回车后会在登录用户的家目录下生成一个.ssh 的目录，此目录下存在id\_rsa私钥与id\_rsa.pub公钥。且讲公钥发布至代码发布的目标服务器上ssh-copy-id -i /root/.ssh/id\_rsa.pub root@IP。

SSH Server配置

name:需要将php程序发布到目标服务器的名称，可自定义

Hostname:填写目录服务器的IP地址

Username:使用那个用户进行发布，此处为进行密钥互信的用户

Remote Directory：此出为发布到目标服务器的相对根路径，建议填写/，防止后续填写路径异常。

注：如果为多台目标服务器，可以继续添加，如果目标服务器存在代理，也可设置proxy

4.3 构建项目

新建Item—填入项目名称—选择构建一个自由风格的软件项目—确定

源码管理选择git

Repository URL 填写具体git上的仓库url，如果为私有，需要继续添加Credentials，如果为公有直接填写url即可，Credentials为none，

构建后够操作

选择（Send files or execute commands over SSH）

SSH Server选择目标服务器如：php-server

Source files：/ \#将git拉去下来的原始文件

Remote directory：/var/www/html \#发布到目标服务器的制定目录

Exec command：chown apache:apache -R /var/www/html/\* \#制定后续的操作

此时可以选择Editable Email Notification来构建邮件通知。

在此处，之前的邮件主题，内容均可以自定义，在高级里面，选择邮件接受人。

点击保存，并立即构建，可以点击console output查看日志

此时打开php程序发现程序文件已经成功发布到目标服务器上

此时可以查看邮件也已经发送成功。

