
<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/d3f8b06f-db5c-442d-ba93-ca2795475fc0" />

Nmap端口扫描

<img width="415" height="235" alt="image" src="https://github.com/user-attachments/assets/cc5a4665-5510-4bc0-a525-fa10d1bca130" />

开放了80和22端口
将 analytical.htb 添加到/etc/hosts
访问web页面

<img width="415" height="240" alt="image" src="https://github.com/user-attachments/assets/557bb520-37dc-4b88-90bd-5386b21c9c99" />

<img width="415" height="240" alt="image" src="https://github.com/user-attachments/assets/e14f33b6-7a86-42d2-a891-87b660f01ccc" />

存在login页面
搜索metabase可利用漏洞

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/ff2d263e-3818-424a-8783-d32644abf651" />

CVE-2023-38646

https://nvd.nist.gov/vuln/detail/CVE-2023-38646

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/57de8d04-6892-49fd-b0e3-af9c2d2ba47a" />

Metabase 开源版本 0.46.6.1 之前版本和 Metabase Enterprise 1.46.6.1 之前版本允许攻击者以服务器权限级别在服务器上执行任意命令。利用该漏洞无需身份验证。其他已修复的版本包括 0.45.4.1、1.45.4.1、0.44.7.1、1.44.7.1、0.43.7.2 和 1.43.7.2。

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/eac87822-5f88-4266-a172-bdf7c0eff22a" />

漏洞利用
访问/api/session/properties目录获取setup-token

<img width="415" height="240" alt="image" src="https://github.com/user-attachments/assets/e35bb37f-9e3c-4c6d-be25-14c2b200ea04" />

python3 main.py -u http://[targeturl] -t [setup-token] -c "[command]"

本地监听端口等待反弹shell

<img width="415" height="117" alt="image" src="https://github.com/user-attachments/assets/674d9e05-bdff-4e8c-a194-cc13a6616275" />

<img width="415" height="236" alt="image" src="https://github.com/user-attachments/assets/b77cf6fd-80cc-4a7d-9ba5-c0d4ed469396" />

没有userfalg

信息收集到环境变量
/proc/self/environ

<img width="416" height="25" alt="image" src="https://github.com/user-attachments/assets/3dcf5c13-620b-4bb0-a4a7-cab76caf4b32" />

找到了metalytics用户的密码
由于在dockers环境中无法su
利用ssh登录获取user-flag

<img width="413" height="410" alt="image" src="https://github.com/user-attachments/assets/aa069406-864b-4524-a165-41d3c25df8a1" />

<img width="415" height="240" alt="image" src="https://github.com/user-attachments/assets/44834faa-c9c2-4341-a0df-4ad1fbce9ef8" />

内核版本存在提权漏洞
#25~22.04.2-Ubuntu

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/554c1675-de58-41eb-9618-fc76203534b0" />

CVE-2023–2640

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/31f01db2-5cde-407e-8108-25744e3e2efd" />

<img width="416" height="134" alt="image" src="https://github.com/user-attachments/assets/2b0d88fb-c429-4948-bf83-ef12a3fb06ce" />

