
<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/51106435-1a38-4d71-a527-1242b9126ee7" />

Nmap端口扫描

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/6d71b976-ea67-485b-86ca-d2284b83cf29" />

这是一台Windows机器
将mailing.htb添加到etc/hosts文件里

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/b597aa82-22ed-415c-8ffd-3ed50bcca011" />

访问web页面

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/457cb2d1-ff26-4de3-8090-203f0b2138f0" />

只有一个下载功能

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/0c85b39d-a53a-4f56-b7d1-2fe58db95a7e" />

尝试文件下载

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/16ea800e-334a-4bab-9f0a-bcae839a9ee1" />

我这里并没有利用成功
尝试SMB枚举
但是没有权限访问

查看了hmailserver的搭建

<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/0f926c51-ba84-458a-a3a9-aa0b373a4791" />

C:\Program Files (x86)\hMailServer\Bin\hMailServer.ini
可能存在一些hash信息

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/9f64c2fd-bd3f-40c2-a5a5-e06f82a16494" />

存在NTLMhash

<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/06b8d8ef-e44c-4b5f-b597-970c5bdd1074" />

CVE-2024-21413

<img width="416" height="314" alt="image" src="https://github.com/user-attachments/assets/5094f7db-d207-4d7d-a4d0-09565a508d10" />

John破解hash

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/2610e97e-002b-4b8f-8e43-54cd52b1cfa5" />

使用evil-winrm连接

<img width="387" height="310" alt="image" src="https://github.com/user-attachments/assets/a99c8773-fd2c-4c42-ba17-9536590bc5ed" />

拿到userflag


<img width="393" height="314" alt="image" src="https://github.com/user-attachments/assets/97f1c5e6-11b6-45a4-8049-38fe0cf304f1" />

4d12779d4c5b915051d10a9983628cff
信息收集发现有一个librepffice
谷歌一波发现存在CVE-2023-2255

<img width="416" height="314" alt="image" src="https://github.com/user-attachments/assets/8292ab73-ba26-4c85-80af-8186b41eb86b" />

利用EXP生成一个odt文件

<img width="415" height="323" alt="image" src="https://github.com/user-attachments/assets/4e2934e0-5392-468a-a07f-cc2f80eb5180" />

利用python3 -m在攻击机开启一个httpserver端口
在目标主机的C:\Important Documents 利用curl -o 命令来下载我们生成的odt文件

<img width="415" height="323" alt="image" src="https://github.com/user-attachments/assets/053d8cdd-f7f0-442e-a053-6a509bc054d5" />

<img width="415" height="323" alt="image" src="https://github.com/user-attachments/assets/2bbe0f0e-e6d1-4704-b510-b19b7df48d7a" />

提到administrator后

用crackmapexec来爆破密码

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/b6fc4e72-aacb-440e-96df-9c3aa6e626fa" />

获得administrator的hash
利用psesc加上hash连接

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/9c2aff31-8399-4815-9d6a-f3c166496e1c" />

拿到rootflag

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/6d9ba6bd-aed6-4c07-9396-5452121b56ab" />












