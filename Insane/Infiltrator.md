
<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/dcc15386-1746-4a80-9734-0cce57176a81" />

<img width="416" height="220" alt="image" src="https://github.com/user-attachments/assets/cac922bd-2728-4414-ac1d-d262b2dda32b" />
开放端口 53,80,88,135,139,389,445,464,593,636,3268,3269,3389,5985,9389,15220,15230,49667,49690,49691,49694,49723,49746,49879

域名 infiltrator.htb
主机 dc01.infiltrator.htb

配置一下/etc/hosts
配置DNS  /etc/resolv.conf 
将这台服务器填到第一行


<img width="265" height="101" alt="image" src="https://github.com/user-attachments/assets/57f55a60-ecf9-4f3f-808b-6dd1c19f489e" />

<img width="383" height="313" alt="image" src="https://github.com/user-attachments/assets/89669d41-58c2-4df4-89ba-fd79cbe4db18" />

解析成功

浏览web页面

<img width="416" height="236" alt="image" src="https://github.com/user-attachments/assets/78ba0e0c-5a97-4ed1-9e1e-a37849de1c74" />

我们发现七个用户名

<img width="325" height="263" alt="image" src="https://github.com/user-attachments/assets/f1c332a5-b330-4a33-8fc3-b132820a2fea" />

根据前面NMAP扫描的135，445端口，
可以尝试用enum4linux来信息探测
Enum4linux是一个用于枚举来自Windows和Samba系统的信息的工具
Enum4linux参考文章： 
https://hackfun.org/2016/10/23/Kali-Linux%E4%BF%A1%E6%81%AF%E6%94%B6%E9%9B%86%E4%B9%8Benum4linux/

enum4linux -a infiltrator.htb 做所有简单的枚举

<img width="415" height="192" alt="image" src="https://github.com/user-attachments/assets/a2b30429-5b7e-4feb-9292-96d431d9bf3d" />

并没有找到什么信息
尝试利用kerbrute从域外对域内进行用户枚举
Kerbrute参考文章:
https://3gstudent.github.io/%E6%B8%97%E9%80%8F%E6%8A%80%E5%B7%A7-%E9%80%9A%E8%BF%87Kerberos-pre-auth%E8%BF%9B%E8%A1%8C%E7%94%A8%E6%88%B7%E6%9E%9A%E4%B8%BE%E5%92%8C%E5%8F%A3%E4%BB%A4%E7%88%86%E7%A0%B4

利用刚才从web页面获取到的七个用户名

<img width="416" height="150" alt="image" src="https://github.com/user-attachments/assets/0d287cfa-00fb-4c48-9dd3-301646b2dee2" />

没有可以利用的信息
尝试换一种组合名字
<img width="221" height="355" alt="image" src="https://github.com/user-attachments/assets/09dcb94e-397b-4c9d-a944-af5738bd47b3" />

<img width="416" height="203" alt="image" src="https://github.com/user-attachments/assets/ba4893f6-a21e-4e5c-be3d-8f3baa87f29f" />
换了一种组合方法，碰撞出了七个用户

<img width="325" height="266" alt="image" src="https://github.com/user-attachments/assets/b03ea7c2-f5f6-48dd-bc35-b85190599297" />

利用GetNPUsers来获取kerberos票证

获取不需要 Kerberos 预身份验证”的域用户，并在不知道其密码的情况下请求其 TGT。然后可以尝试破解随票证发送的会话密钥以检索用户密码
impacket-GetNPUsers infiltrator.htb/ -usersfile users.txt -outputfile out.txt -dc-ip dc01.infiltrator.htb -no-pass
参考文章：
https://tools.thehacker.recipes/impacket/examples/getnpusers.py

<img width="416" height="180" alt="image" src="https://github.com/user-attachments/assets/6a335741-8f37-4dcb-91c6-58d64d1c4373" />

获取到l.clark@infiltrator.htb用户的票据

利用hashcat来进行密码碰撞
<img width="416" height="123" alt="image" src="https://github.com/user-attachments/assets/726b1521-df59-48a2-a6be-bdb38e031f29" />

<img width="416" height="167" alt="image" src="https://github.com/user-attachments/assets/be08c2fa-5d60-40cc-8193-290a1f8d54ca" />

成功爆破出
l.clark@infiltrator.htb
WAT?watismypass!

使用crackmap进行密码喷洒

<img width="353" height="206" alt="image" src="https://github.com/user-attachments/assets/bdb49d64-9475-4d6e-8d49-b23ca25e9254" />

喷洒失败，这里如果使用美国节点的话就可以喷洒成功，应该是HTB的问题

尝试获取d.anderson的身份票据

impacket-getTGT infiltrator.htb/d.anderson:'WAT?watismypass!' -dc-ip dc01.infiltrator.htb
使用getTGT来获取来获取d.anderson用户的TGT票据

<img width="341" height="86" alt="image" src="https://github.com/user-attachments/assets/d69cb4e6-45be-4678-8b39-7b705376bb3d" />
获取成功




