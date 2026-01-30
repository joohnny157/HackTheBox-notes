<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/27ce9f98-a125-4b3a-89bd-1fbfa451b475" />

Nmap端口扫描，开放了 21 22 80端口

<img width="415" height="235" alt="image" src="https://github.com/user-attachments/assets/e8423fed-f396-4de0-bb22-c01b519995fc" />

访问80端口web页面

<img width="416" height="232" alt="image" src="https://github.com/user-attachments/assets/2bf31a1a-6af4-461e-81d9-4419a579039f" />

存在文件上传点

<img width="416" height="231" alt="image" src="https://github.com/user-attachments/assets/0e9f1f7c-082d-4bbf-b2b8-84191043ce99" />

文件显示上传成功，但是并没有执行代码

Gobuster目录扫描只发现一个status的目录

<img width="415" height="235" alt="image" src="https://github.com/user-attachments/assets/edf482b2-4acf-4d93-acaa-db45d85d1893" />

<img width="415" height="225" alt="image" src="https://github.com/user-attachments/assets/84ba2f02-5d56-4fe7-bd21-6042ad4a2487" />

并没有什么可以利用的点

Wfuzz子域名扫描

<img width="416" height="196" alt="image" src="https://github.com/user-attachments/assets/00cd60a0-1fe0-4586-add1-baa7a46dc9ca" />

发现存在一个admin的子域名
将他添加到etc/hosts文件中

<img width="415" height="232" alt="image" src="https://github.com/user-attachments/assets/a6a11ec8-1670-4f38-8d02-37713500a8e5" />
显示，只能本地访问

回到刚才的文件上传点

<img width="415" height="233" alt="image" src="https://github.com/user-attachments/assets/1c4e77dd-2570-4a64-bad5-a6fe61b5ba64" />

显示可以本地上传文件，也可通过http，https协议访问其他网站的文件


尝试访问攻击机webshell文件

<img width="415" height="234" alt="image" src="https://github.com/user-attachments/assets/6d0873eb-2bd1-405d-b52c-48fdf8d3b437" />

攻击机开启一个http的端口

<img width="416" height="68" alt="image" src="https://github.com/user-attachments/assets/73e76dc6-9e6b-46a6-8490-641ecc2d4d1b" />

<img width="415" height="231" alt="image" src="https://github.com/user-attachments/assets/0bf1f240-e5cc-4f4f-a634-dd3ec522c5d7" />

<img width="416" height="232" alt="image" src="https://github.com/user-attachments/assets/8ac23230-4c1a-4730-a012-60dea4b0e2fa" />

反弹shell失败

我们在这里可以确定的是，这台服务器可以访问外部服务器文件

<img width="416" height="60" alt="image" src="https://github.com/user-attachments/assets/d694e3ce-5205-46a3-92a3-0646bd292a32" />

但是目前没有办法执行命令

尝试访问http://admin.forge.htb

<img width="415" height="169" alt="image" src="https://github.com/user-attachments/assets/3c108fc9-06ca-47b2-990f-7804756ed8cc" />

因为只允许localhost访问，所以是黑名单IP
尝试大小写绕过

<img width="416" height="235" alt="image" src="https://github.com/user-attachments/assets/91ade24f-135a-470e-9c7d-86c05f681be6" />

绕过成功
执行curl有数据返回

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/1921fd8f-8bf6-4171-9292-36ea0b5faf0c" />

并且提示有关/announcements目录的信息
尝试访问/announcements目录

<img width="416" height="205" alt="image" src="https://github.com/user-attachments/assets/d2c69052-ce31-4aaf-80a6-903ee639fb46" />

我们得到了一个FTP服务的用户 
heightofsecurity123!

<img width="415" height="328" alt="image" src="https://github.com/user-attachments/assets/b1f9c4cb-2475-42d5-a8e6-b6ad809c8cc1" />

可以通过upload功能与FTP进行交互

在前期NMAP扫描的时候发现开放了21 FTP端口
尝试在URL框添加FTP协议来访问FTP服务

<img width="415" height="231" alt="image" src="https://github.com/user-attachments/assets/03d1c611-d13b-413e-9c28-ed2248fb81d6" />

<img width="415" height="65" alt="image" src="https://github.com/user-attachments/assets/84e5ef27-1e37-46df-ba57-6d9ffd7a905e" />

访问成功

http://ADMIN.FORGE.htb/upload?u=ftp://user:heightofsecurity123!@127.0.1.1/

现在可以成功访问文件了
尝试访问ssh私钥

一般是在用户目录下的/.ssh/id_rsa

<img width="415" height="204" alt="image" src="https://github.com/user-attachments/assets/e12985e8-e642-4301-b66c-92e4e8badb79" />

<img width="415" height="221" alt="image" src="https://github.com/user-attachments/assets/b2fd35c2-b34a-4495-b0d8-62dce18b02bf" />

通过user用户，连接成功
Sudo -l 查看sudo文件

<img width="416" height="79" alt="image" src="https://github.com/user-attachments/assets/c7987354-ea71-4bb2-8a94-4e3e14e7fc04" />

分析脚本

<img width="416" height="226" alt="image" src="https://github.com/user-attachments/assets/80e4c7d9-095d-40b4-a150-38712fe9b0d4" />

它会产生一个随机的端口来侦听，确定密码是否为secretadminpassword
如果是的话，他就会打开python调试器
为了完成权限提升操作，我们需要两个shell会话

<img width="415" height="131" alt="image" src="https://github.com/user-attachments/assets/e49fbe49-fca2-4805-ad96-60baa6e3b73b" />

在shell1执行利用sudo 执行这个文件会产生一个随机的端口

<img width="415" height="128" alt="image" src="https://github.com/user-attachments/assets/871de53a-e7a9-45ce-acfd-69997373a358" />

Shell2监听端口，输入密码进入调试

<img width="416" height="115" alt="image" src="https://github.com/user-attachments/assets/5e1024f9-dad8-46e4-8a92-be43f3a5d42d" />

在shell2处输入os.system，这是用来调用系统命令，因为这个文件是以root身份执行的，那我们在调用系统命令的时候就是root身份

Shell1处进入调试器，这里只需要os模块调用shell会话即可

import os;os.system("/bin/sh")

