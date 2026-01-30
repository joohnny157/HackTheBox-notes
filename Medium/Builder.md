
<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/ba07b564-2d8f-40e0-b1e3-1508e3266c8a" />

这里Nmap扫描发现只存在22端口和8080端口

<img width="387" height="307" alt="image" src="https://github.com/user-attachments/assets/464304c7-a3a7-42ce-a73a-ea40cc598576" />

进去之后发现是一个Jenkins搭建的

<img width="416" height="259" alt="image" src="https://github.com/user-attachments/assets/50f3eea7-45a3-467b-9880-e5067d2c7107" />

Jenkins版本号是 2.441

<img width="415" height="188" alt="image" src="https://github.com/user-attachments/assets/711629d3-65fe-40fc-9793-277c5e98163c" />

通过Google搜索发现该版本存在CVE-2024-23897

<img width="416" height="259" alt="image" src="https://github.com/user-attachments/assets/901df3f1-1136-4cbc-9966-70adb724c7f2" />

通过git clone下载到本地
查看该利用CVE描述
我们需要下载目标主机上的命令执行客户端
wget http://10.10.11.10:8080/jnlpJars/jenkins-cli.jar

<img width="416" height="260" alt="image" src="https://github.com/user-attachments/assets/e5365fb8-73f9-4feb-bdb8-5a21e89148f7" />

java -jar jenkins-cli.jar -s http://10.10.11.10:8080/ -http connect-node "@/etc/passwd"  
双引号里面的内容就是你要执行的命令 但是好像只能读取文件
反弹shell失败
我们先来查看一下/etc/passwd

<img width="391" height="306" alt="image" src="https://github.com/user-attachments/assets/4c9a60c4-076d-4920-863f-41e0c3efc6bd" />

发现了一个叫jenkins的用户
读取变量进程文件 发现其主文件夹位于
/var/jenkins_home

<img width="377" height="134" alt="image" src="https://github.com/user-attachments/assets/741b68eb-cf53-4c32-8829-d353cdda8728" />

成功获取user.txt

<img width="381" height="297" alt="image" src="https://github.com/user-attachments/assets/61d40777-35a9-48c5-958c-e8106359c3bb" />

查看/var/jenkins_home/users下的users.xml
发现存在一个Jennifer用户

<img width="387" height="300" alt="image" src="https://github.com/user-attachments/assets/4a084761-aee0-4425-8c3b-f9ee468b4774" />

查找他的config文件

<img width="402" height="351" alt="image" src="https://github.com/user-attachments/assets/a876a064-647a-4fec-af3a-ef0e89a15f83" />

发现末尾还存在一个hash密码

<img width="416" height="394" alt="image" src="https://github.com/user-attachments/assets/7366849e-6e47-45be-bd79-6ab9e263b541" />

将冒号后面的内容复制出来到一个文件里面
利用john来破解这段hash

<img width="388" height="312" alt="image" src="https://github.com/user-attachments/assets/9ec0e58d-904c-4ba5-8d27-b9fb111aa2c6" />

<img width="391" height="309" alt="image" src="https://github.com/user-attachments/assets/ff1d0374-6920-4fea-9649-23f4aeb9d275" />

破解密码为princess
我尝试用该密码登陆jbcrypt的ssh账户，但尝试失败

<img width="387" height="305" alt="image" src="https://github.com/user-attachments/assets/caaa2c76-66b1-4ab5-97cf-a1890f48eed7" />

现在可以回到web页面

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/bef1435c-11bb-4919-acd9-61c67d9b9252" />

可以用该密码登陆jennifer用户
来到manage jenkins 往下滑找到Script Console

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/5602f144-11b9-43f2-91f7-980649a5e09d" />

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/f8d1e430-bde2-49a1-8f97-abe65f236800" />

通过一波Google发现这个地方可以执行命令

<img width="416" height="262" alt="image" src="https://github.com/user-attachments/assets/77a61251-dfb0-4b9c-aac7-9b95f341ecc1" />

反弹shell

<img width="388" height="309" alt="image" src="https://github.com/user-attachments/assets/73cf6b2f-3cc6-4340-b842-ac7940a7372e" />

反弹失败

<img width="416" height="262" alt="image" src="https://github.com/user-attachments/assets/f9ab137e-473c-4783-8bc5-910d1afc2746" />

我们将反弹shell的命令写入到一个脚本里面
接着利用python起一个http.server的端口
之后利用命令这里的命令执行来下载我们攻击机上的反弹shell脚本

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/cafb8219-d991-41a9-bfd1-e49f39377bf0" />

<img width="416" height="358" alt="image" src="https://github.com/user-attachments/assets/fafb745d-14a4-4d83-88f7-be2c3afd9bd7" />

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/bef8cc51-2a30-4487-a4a4-7b72770ee988" />

利用curl -o 成功下载到目标主机的/tmp目录下
本地攻击机监听3333端口
目标主机执行反弹shell脚本

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/002273b1-2104-4060-a7ea-0b704695fc4a" />

成功获取到shell
在/var/jenkins_home目录下找到了credentials.xml文件
里面包含了ssh私钥

<img width="388" height="312" alt="image" src="https://github.com/user-attachments/assets/9e9a1802-7ecc-4db0-bc48-92df2564e2cd" />

<img width="389" height="307" alt="image" src="https://github.com/user-attachments/assets/14771578-2463-48f5-baeb-4cb41eb58a1b" />

将这一坨复制出来
利用上面给出的语法来进行破解

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/6823da9a-2a0f-4a54-82dc-9fb2172456f5" />

也可以利用
https://gist.github.com/hoto/d1c874480888f8711f12db33a20b6e4d
这个脚本来破解
将破解后的那一大坨全部复制到一个文件里面

<img width="416" height="358" alt="image" src="https://github.com/user-attachments/assets/d60d3ddd-4760-408a-8089-4326d9e6c35a" />

chmod 600 shit
ssh -i shit root@10.10.11.10

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/680130af-5a63-4fef-b76b-859007ca281d" />

成功获取root权限

<img width="415" height="212" alt="image" src="https://github.com/user-attachments/assets/ceec0b2f-95b2-4c2d-bdf6-f414b4cc50c3" />
