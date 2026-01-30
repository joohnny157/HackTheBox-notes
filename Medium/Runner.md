
<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/84bf91cd-2fe6-44b9-86f7-886c77053dc7" />

Nmap端口扫描
开放了 22 20 8000端口

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/f45dfc26-0b77-4117-ba03-b251dfa3ee8b" />

将域名添加到/etc/hosts中

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/a8265a7e-ff00-43b0-8b1a-de27899224a7" />

访问web页面

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/117f77da-3164-46a7-89c8-e3861cca14a0" />

Whatweb 

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/277aaa07-8bf5-4ad2-8cf5-c540c08e8659" />

gobuster爆破一下子域名

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/ddd723f9-329b-4c1e-a06b-80b0de595cbb" />

<img width="251" height="16" alt="image" src="https://github.com/user-attachments/assets/0c74243a-1066-4133-913e-8e3a98173458" />

老样子 添加到/etc/hosts

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/ed8d4f60-3be7-4aa7-9237-de4c8a9c12de" />

访问

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/202b26d5-a73f-4ec7-acad-b6aaa49b4a52" />

<img width="144" height="25" alt="image" src="https://github.com/user-attachments/assets/d83b2f8a-1cc6-4a6e-9089-3226a6fa87d5" />

版本号有了
上exploit-db

<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/74082788-7fd1-4b62-90c1-3b86fa0f06e2" />

运行这个脚本文件
得到账号密码

<img width="384" height="308" alt="image" src="https://github.com/user-attachments/assets/cfed7fe3-08a7-4d5d-9331-5c109b3008ea" />

登陆成功

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/17de863a-8c16-4255-9b4b-001961f7df8b" />

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/9ee8a3bc-ab6c-4e8a-9541-280c39458b9f" />

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/95711e33-56a5-45ac-b2d1-8ac3994f5ac0" />

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/a1e96a67-0ee2-4209-8cd4-f730f6a33102" />

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/f9b4cc2f-3efe-4f7b-a60a-d8df9eff242b" />

在backup里有个备份文件 下载
解压
在config/projects/AllProjects/pluginData/ssh_keys/中有一个id_rsa文件

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/c84a018c-82a1-4bf6-8528-8adc02905863" />

Database dump中有一个user文件

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/cc5d9cdf-7279-4cff-b901-b0c3f4dd6fae" />

尝试破解hash

<img width="388" height="304" alt="image" src="https://github.com/user-attachments/assets/09248f7d-bb01-4633-aec4-56faa152bcc2" />

破解成功，但是ssh连接失败
只能从id_rsa下手
但是我不知道这个文件的用户名是谁 难搞

我在http://teamcity.runner.htb/admin/admin.html?item=users
找到了一些用户名
一个一个试

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/a658ff14-b449-42ec-a7d6-aa8ce417cf8d" />

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/d0f68e89-b26d-4bef-910a-bde708b76a3a" />

第一个就进来了
拿到userflag

<img width="387" height="307" alt="image" src="https://github.com/user-attachments/assets/37874556-b1b2-4241-b859-cc584724af9c" />

上传
执行

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/359f214d-006a-46d3-b7b2-7a4136b0decb" />

<img width="393" height="313" alt="image" src="https://github.com/user-attachments/assets/5d0c6dde-c257-4e63-9af1-52a47a478897" />

又有一个子域名

<img width="388" height="325" alt="image" src="https://github.com/user-attachments/assets/82e0ac0a-a7d3-4040-aa89-44f8b450ead3" />

9000端口也开放了一个服务

Chisel隧道

<img width="416" height="166" alt="image" src="https://github.com/user-attachments/assets/9fb2827b-a1f7-4221-a98b-39aafc7cc9b3" />

将目标主机本地的9000端口转发到我们攻击机的9000端口
访问127.0.0.1：9000

<img width="415" height="195" alt="image" src="https://github.com/user-attachments/assets/b187894f-adea-4ff9-9dc9-c05fba94406a" />

利用我们刚才hash破解的matthew账号登陆

<img width="415" height="205" alt="image" src="https://github.com/user-attachments/assets/ac8364df-d970-4860-b381-cf756e6500d6" />

<img width="415" height="202" alt="image" src="https://github.com/user-attachments/assets/7762dc46-435b-48e4-a4ff-2fa1afd51834" />

<img width="416" height="252" alt="image" src="https://github.com/user-attachments/assets/fa5006b7-e6d8-4d82-bc9d-f1652c30bf4b" />

Image填上teamcity:lastest

<img width="415" height="247" alt="image" src="https://github.com/user-attachments/assets/d1e41ef7-7606-42cb-a6f3-449441e3156d" />

Working dir填上/proc/self/fd/8

<img width="415" height="205" alt="image" src="https://github.com/user-attachments/assets/e7cf29ab-d6d3-410f-a136-57e4987b5593" />

点进去

<img width="415" height="202" alt="image" src="https://github.com/user-attachments/assets/c5dfad9f-d678-413d-982d-68037def9cca" />



获得rootflag


Docker提权参考
https://rioasmara.com/2021/08/15/use-portainer-for-privilege-escalation/







