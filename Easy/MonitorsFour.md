<img width="1280" height="674" alt="Image" src="https://github.com/user-attachments/assets/5b631468-ca25-4d06-afe5-cdb59b714903" />

Nmap扫描到80端口
将域名绑定到 /etc/hosts

nmap -sC -sV 10.129.4.120

echo 10.129.4.120 monitorsfour.htb >> /etc/hosts

<img width="572" height="203" alt="Image" src="https://github.com/user-attachments/assets/70f611ec-5617-4eb1-ae55-f17c74b63664" />

ffuf目录扫描 ，发现login，user，forgot-password
ffuf -u http://monitorsfour.htb/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -mc 200


URL访问，提示少了token参数

<img width="357" height="105" alt="Image" src="https://github.com/user-attachments/assets/f7167d10-b4f4-45cd-88ee-8bc7dcfb2289" />


加上token参数，值为0
出来了一些账号密码

<img width="357" height="105" alt="Image" src="https://github.com/user-attachments/assets/ece9f07b-97da-4cea-9256-819211f40ac1" />

子域名搞一下
出来了一个cacti

添加到hosts
ffuf -u http://monitorsfour.htb -H "Host: FUZZ.monitorsfour.htb" -ac -w /usr/share/wordlists/amass/subdomains-top1mil-5000.txt

echo 10.129.4.120 cacti.monitorsfour.htb >> /etc/hosts

<img width="917" height="401" alt="Image" src="https://github.com/user-attachments/assets/d6bf3a88-1f96-40ce-8a49-531ffd493112" />

将之前获取到的admin的 hash解密
得到 amdin ：wonderful1

<img width="701" height="305" alt="Image" src="https://github.com/user-attachments/assets/9d3be75b-ba93-4a86-93d1-ced42cbf39b4" />

版本是1.2.28

<img width="291" height="227" alt="Image" src="https://github.com/user-attachments/assets/8da473f9-2686-4a47-a2f5-0326309e5a51" />

但是admin账号被禁用，无法登录

使用marcus ：wonderful1

该版本存在CVE-2025-24367
使用exp攻击
https://github.com/TheCyberGeek/CVE-2025-24367-Cacti-PoC/blob/main/exploit.py

<img width="609" height="155" alt="Image" src="https://github.com/user-attachments/assets/c11c515b-4ce3-4803-9796-7adb0f611fc7" />

<img width="609" height="155" alt="Image" src="https://github.com/user-attachments/assets/4794618c-da46-44ce-99c9-c0de645f69b7" />

拿到userflag

<img width="608" height="441" alt="Image" src="https://github.com/user-attachments/assets/77537693-157b-4fd7-b240-972263b01483" />

当前是在docker环境中

<img width="330" height="54" alt="Image" src="https://github.com/user-attachments/assets/72f3f5c7-d152-4a2b-8ba9-05ebc15fc4eb" />

Fscan扫到有一个docker api未授权RCE

<img width="473" height="266" alt="Image" src="https://github.com/user-attachments/assets/d8bd8bc5-cc73-4110-8809-5b9462f150aa" />

直接用大佬的EXP
https://blog.csdn.net/qq_43007452/article/details/156382525

<img width="537" height="306" alt="Image" src="https://github.com/user-attachments/assets/7cc1e28b-54ca-4026-889c-faaed9decc14" />
