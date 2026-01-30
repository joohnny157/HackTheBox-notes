
<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/416d55d3-4d94-4907-b560-09d2dbe26488" />

Nmap端口扫描开放了25，53，80，88，135，445，464，593，636，3268，3269端口

<img width="415" height="135" alt="image" src="https://github.com/user-attachments/assets/182ebf90-461c-419a-baec-8adfd343ca9d" />

<img width="416" height="394" alt="image" src="https://github.com/user-attachments/assets/ce1bde9d-4b05-48e7-9960-77b5a5d9e682" />

不存在smb匿名访问

<img width="415" height="148" alt="image" src="https://github.com/user-attachments/assets/59c4fa18-7157-4788-bf0b-a005f5b27725" />

目录扫描和子域名枚举也没什么东西

<img width="415" height="439" alt="image" src="https://github.com/user-attachments/assets/9b27b346-c3bd-474f-98cd-0b6ade4b42ad" />

通过邮件服务器尝试发送payload，这里可以看到txt文件发送成功

https://swisskyrepo.github.io/InternalAllTheThings/redteam/access/office-attacks/#docm-macro-creator

XLL-EXEC利用

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/ae8f80f5-8d56-491e-90e7-b7978d18a232" />

将里面的CMD替换成powershell的反弹shell，需要base64编码之后的反弹shell

<img width="416" height="323" alt="image" src="https://github.com/user-attachments/assets/03f3b1d1-623a-48fb-bcd9-359adcdfbc95" />

<img width="415" height="103" alt="image" src="https://github.com/user-attachments/assets/1ce3c7ac-eb93-485e-afe1-0b9e0a50db74" />

<img width="416" height="41" alt="image" src="https://github.com/user-attachments/assets/2f662727-cabd-474b-95fa-90acd3e3c4eb" />

将exp.c编译成shell.xll

这里反弹shell失败


尝试修改执行命令

<img width="321" height="267" alt="image" src="https://github.com/user-attachments/assets/1d487d33-ed66-4646-a42c-90a312a5fc5d" />

执行命令，让他来下载攻击机上的payload文件

<img width="323" height="274" alt="image" src="https://github.com/user-attachments/assets/166b3352-a101-44e6-bd5b-2fb238a25c98" />

<img width="416" height="33" alt="image" src="https://github.com/user-attachments/assets/dc9efe3b-112f-4c1a-bf89-8f5c08fcb47e" />

将他编译成john.xll

1、开放9999端口

<img width="416" height="84" alt="image" src="https://github.com/user-attachments/assets/566a6602-712b-4100-8ac6-68aa6720df24" />

2、监听8888反弹shell端口

<img width="416" height="50" alt="image" src="https://github.com/user-attachments/assets/e3a6b1c7-0d82-4320-87cb-be0e5f18c63d" />

3、执行swaks --to accounts@axlle.htb --from johnny@johnny.test --header "Subject: 1234" --body "shell test"  --attach @john.xll

<img width="415" height="393" alt="image" src="https://github.com/user-attachments/assets/9d7365fb-7728-4692-9bae-d61cacf7c570" />


4、拿到shell

<img width="383" height="52" alt="image" src="https://github.com/user-attachments/assets/054977da-f2c7-4baa-bf78-52fd34ac7fd6" />

在C:\Program Files (x86)\hmailserver\data\axlle.htb\dallon.matrix\2F中有一个文件

<img width="316" height="56" alt="image" src="https://github.com/user-attachments/assets/9d9f8a07-15ff-4523-a5c7-192fd9624b79" />

利用johnny.url文件执行shell.hta
我在shell.hta里面的命令是，执行powershell命令来下载我攻击机上的msfshell
Johnny.url必须放在C:\inetpub\testing下
URL的内容需要指向shell.hta文件所在的目录

<img width="316" height="56" alt="image" src="https://github.com/user-attachments/assets/856b29bf-bee9-46a1-b4dc-09af4db00f71" />

<img width="320" height="175" alt="image" src="https://github.com/user-attachments/assets/a5f8d5a8-f4bb-432d-a5e5-365c2eb279ad" />
