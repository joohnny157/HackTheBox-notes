
<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/190e99a3-54c6-4026-95b0-bfdd6223c279" />
Nmap端口扫描，开放了 22，80，3000，3306端口

<img width="416" height="218" alt="image" src="https://github.com/user-attachments/assets/21016107-17f3-4aed-b839-d7234f16b458" />

3000端口开放 grafana8.2.0版本web应用服务

<img width="415" height="232" alt="image" src="https://github.com/user-attachments/assets/bc83d70d-597d-4e14-96d1-2b235bfd7ee0" />

Gobuster目录扫描并没有发现什么

<img width="416" height="167" alt="image" src="https://github.com/user-attachments/assets/c6789b77-5854-40e4-a198-d253aa19dc54" />

Google搜索grafana8.2.0 exploit
漏洞指向CVE-2021-43798

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/db72c72d-25f2-4f3f-b86f-afe8ce1e5a41" />

分析该漏洞编号

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/9206c403-8dc6-4546-ad8a-dd9c10829ce5" />

此漏洞为未授权任意文件读取漏洞

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/b82323f0-6b02-49e3-b4ed-3066d3139989" />

Payload为
http://localhost:3000/public/plugins/alertlist/../../../../../../../../etc/passwd

<img width="415" height="228" alt="image" src="https://github.com/user-attachments/assets/ae220116-905e-4537-8631-ea43534b46f7" />

搜索granafa配置文件

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/4fffd811-3a16-4367-a033-a6799967ac7a" />

/etc/grafana/grafana.ini

<img width="415" height="230" alt="image" src="https://github.com/user-attachments/assets/ab643d02-5414-4b52-86ee-802172516f3c" />

发现admin密码 messageInABottle685427

利用账号密码，成功登录 grafana平台

<img width="415" height="233" alt="image" src="https://github.com/user-attachments/assets/a112d7a6-42a9-46f0-b2bb-52eb319d3fb0" />

读取数据库配置文件 /var/lib/grafana/grafana.db

找到grafana mysql密码 dontStandSoCloseToMe63221!

<img width="415" height="230" alt="image" src="https://github.com/user-attachments/assets/dc4c033f-ec1b-4e13-9c58-568640dee1e2" />

尝试连接mysql
mysql -u grafana -h 10.129.228.56 -p

<img width="415" height="118" alt="image" src="https://github.com/user-attachments/assets/7ef8a2a3-2fc1-4f25-bce0-21f5d2fb261a" />

拿到developer用户的密码

<img width="415" height="264" alt="image" src="https://github.com/user-attachments/assets/371cedae-1743-4c39-876f-ba9e88bc0726" />

Base64 -d 解码得到明文密码 anEnglishManInNewYork027468

<img width="416" height="267" alt="image" src="https://github.com/user-attachments/assets/03d4fc78-4597-481d-9ac9-67fc34085259" />

Ssh连接登录拿到user-flag

<img width="415" height="215" alt="image" src="https://github.com/user-attachments/assets/d56b8df2-24f2-4407-a085-6720704e936f" />

在/opt/my-all  ls -al

有一个git 执行git show

<img width="416" height="238" alt="image" src="https://github.com/user-attachments/assets/e299cfd0-48f6-4179-816a-2990dbc0db3f" />

拿到token

<img width="416" height="229" alt="image" src="https://github.com/user-attachments/assets/e92be7c8-8e65-42d4-b7ab-e982e56d9241" />

Consul 存在漏洞利用
https://github.com/owalid/consul-rce

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/210e252b-5422-4b51-a578-1f18dbf0bce1" />

端口 8500

利用RCE python脚本

python3 exploit.py -th 127.0.0.1 -tp 8500 -ct bb03b43b-1d81-d62b-24b5-39540ee469b5 -c "chmod +s /bin/bash"


利用python执行脚本本 指向本地8500端口 加上之前获得的APItoken 
-c 加上要执行的命令 
这里
chmod +s /bin/bash

这里我将 /bin/bash赋予SUID权限，所有用户都可以以root身份调用它

当我执行/bin/bash -p之后 我就获得了bash窗口 
拿到root权限

<img width="415" height="191" alt="image" src="https://github.com/user-attachments/assets/96a1cc7d-5876-44be-a634-c8ccb045a64f" />
