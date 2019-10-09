- You must put some 'source' URIs in your sources.list
```
背景 sudo apt-get build-dep python-imaging
编辑 /etc/apt/sources.list

增加 
deb-src http://archive.ubuntu.com/ubuntu trusty main restricted #Added by software-properties
deb-src http://gb.archive.ubuntu.com/ubuntu/ trusty restricted main universe multiverse #Added by software-properties
deb-src http://gb.archive.ubuntu.com/ubuntu/ trusty-updates restricted main universe multiverse #Added by software-properties
deb-src http://gb.archive.ubuntu.com/ubuntu/ trusty-backports main restricted universe multiverse #Added by software-properties
deb-src http://security.ubuntu.com/ubuntu trusty-security restricted main universe multiverse #Added by software-properties
deb-src http://gb.archive.ubuntu.com/ubuntu/ trusty-proposed restricted main universe multiverse #Added by software-properties

执行sudo apt-get update

```
- Could not find a version that satisfies the requirement PIL (from versions: )  No matching distribution found for PIL  
```
PIL名字改变 sudo pip install Pillow 
```

- install Xmind
```
下载途径（破解的话一定要在英文官网下载！）
英文官网：http://www.xmind.net/download/win/

网上破解的帖子有很多。但都缺少关键插件下载。亲测可行。自取
破解：
1. 将破解文件XMindCrack.jar复制到安装根目录， 如: `C:\Program Files (x86)\XMind 8 Update 2\plugins`
2. 以文本格式打开安装目录中 XMind.ini
3. 在 XMind.ini 最后追加 `-javaagent:plugins/XMindCrack.jar`
4. 禁止XMind访问网络
    建议使用防火墙或者host记录法


*防火墙：在控制面板的Windows 防火墙左侧中点击高级设置，然后点击右侧的新建规则，选择程序，点击下一步，选择此程序路径，输入D:\XMind.exe，点击下一步。点击阻止连接，点击下一步。选中域，公用，点击下一步。随便填写个名称，点击完成。
*hosts：找到hosts，在末尾一行添加127.0.0.1 www.xmind.net 【注意中间有空格】


5. 打开xmind 8 输入序列号：
XAka34A2rVRYJ4XBIU35UZMUEEF64CMMIYZCK2FZZUQNODEKUHGJLFMSLIQMQUCUBXRENLK6NZL37JXP4PZXQFILMQ2RG5R7G4QNDO3PSOEUBOCDRYSSXZGRARV6MGA33TN2AMUBHEL4FXMWYTTJDEINJXUAV4BAYKBDCZQWVF3LWYXSDCXY546U3NBGOI3ZPAP2SO3CSQFNB7VVIY123456789012345
插件链接:  https://pan.baidu.com/s/1qZHFJ4o  密码: 7k74
```

- install Intellij
```
https://www.cnblogs.com/wang1024/p/7485758.html
      在激活Jetbrains旗下任意产品的时候选择激活服务器

      填入以下地址便可成功激活

      http://idea.liyang.io
      http://idea.imsxm.com
      http://jetbrains.tech

激活code 

K03CHKJCFT-eyJsaWNlbnNlSWQiOiJLMDNDSEtKQ0ZUIiwibGljZW5zZWVOYW1lIjoibnNzIDEwMDEiLCJhc3NpZ25lZU5hbWUiOiIiLCJhc3NpZ25lZUVtYWlsIjoiIiwibGljZW5zZVJlc3RyaWN0aW9uIjoiRm9yIGVkdWNhdGlvbmFsIHVzZSBvbmx5IiwiY2hlY2tDb25jdXJyZW50VXNlIjpmYWxzZSwicHJvZHVjdHMiOlt7ImNvZGUiOiJJSSIsInBhaWRVcFRvIjoiMjAxOS0wNC0yNSJ9LHsiY29kZSI6IlJTMCIsInBhaWRVcFRvIjoiMjAxOS0wNC0yNSJ9LHsiY29kZSI6IldTIiwicGFpZFVwVG8iOiIyMDE5LTA0LTI1In0seyJjb2RlIjoiUkQiLCJwYWlkVXBUbyI6IjIwMTktMDQtMjUifSx7ImNvZGUiOiJSQyIsInBhaWRVcFRvIjoiMjAxOS0wNC0yNSJ9LHsiY29kZSI6IkRDIiwicGFpZFVwVG8iOiIyMDE5LTA0LTI1In0seyJjb2RlIjoiREIiLCJwYWlkVXBUbyI6IjIwMTktMDQtMjUifSx7ImNvZGUiOiJSTSIsInBhaWRVcFRvIjoiMjAxOS0wNC0yNSJ9LHsiY29kZSI6IkRNIiwicGFpZFVwVG8iOiIyMDE5LTA0LTI1In0seyJjb2RlIjoiQUMiLCJwYWlkVXBUbyI6IjIwMTktMDQtMjUifSx7ImNvZGUiOiJEUE4iLCJwYWlkVXBUbyI6IjIwMTktMDQtMjUifSx7ImNvZGUiOiJHTyIsInBhaWRVcFRvIjoiMjAxOS0wNC0yNSJ9LHsiY29kZSI6IlBTIiwicGFpZFVwVG8iOiIyMDE5LTA0LTI1In0seyJjb2RlIjoiQ0wiLCJwYWlkVXBUbyI6IjIwMTktMDQtMjUifSx7ImNvZGUiOiJQQyIsInBhaWRVcFRvIjoiMjAxOS0wNC0yNSJ9LHsiY29kZSI6IlJTVSIsInBhaWRVcFRvIjoiMjAxOS0wNC0yNSJ9XSwiaGFzaCI6Ijg4MjUwMTQvMCIsImdyYWNlUGVyaW9kRGF5cyI6MCwiYXV0b1Byb2xvbmdhdGVkIjpmYWxzZSwiaXNBdXRvUHJvbG9uZ2F0ZWQiOmZhbHNlfQ==-IJBDUuZMqhMJZfuM8Pgz1WXDRP3k9sKMJXuGdnbwoYDN4Y2G5Xmpw05GZUeESnh2/wzVxTHF4+PQ5ewk+J66F15b50VHSNxFI9XKWatoDfBc9EA1CddWqAU5CaipdMkSHoUDbT69PPGU4Vsfo1HTFv50tQ7RFVYMDbmVhw6mUbTFMvGiu5CZTuEVkmJ+1KpfuyQZvXjS1hXhfbK/xmpMG2/xRmK9lxW9PafZU1dWxqjYU8QBlUYgjdDsDS2apSTE+xFF6y3ZKK/YThpC7IYt5HR5Cc3VGjdb/H7jEAkH2/Uz0PrixPc3Bk5tU01rhxI4Z5VbmmWzGAhWWBtQEqU17A==-MIIEPjCCAiagAwIBAgIBBTANBgkqhkiG9w0BAQsFADAYMRYwFAYDVQQDDA1KZXRQcm9maWxlIENBMB4XDTE1MTEwMjA4MjE0OFoXDTE4MTEwMTA4MjE0OFowETEPMA0GA1UEAwwGcHJvZDN5MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAxcQkq+zdxlR2mmRYBPzGbUNdMN6OaXiXzxIWtMEkrJMO/5oUfQJbLLuMSMK0QHFmaI37WShyxZcfRCidwXjot4zmNBKnlyHodDij/78TmVqFl8nOeD5+07B8VEaIu7c3E1N+e1doC6wht4I4+IEmtsPAdoaj5WCQVQbrI8KeT8M9VcBIWX7fD0fhexfg3ZRt0xqwMcXGNp3DdJHiO0rCdU+Itv7EmtnSVq9jBG1usMSFvMowR25mju2JcPFp1+I4ZI+FqgR8gyG8oiNDyNEoAbsR3lOpI7grUYSvkB/xVy/VoklPCK2h0f0GJxFjnye8NT1PAywoyl7RmiAVRE/EKwIDAQABo4GZMIGWMAkGA1UdEwQCMAAwHQYDVR0OBBYEFGEpG9oZGcfLMGNBkY7SgHiMGgTcMEgGA1UdIwRBMD+AFKOetkhnQhI2Qb1t4Lm0oFKLl/GzoRykGjAYMRYwFAYDVQQDDA1KZXRQcm9maWxlIENBggkA0myxg7KDeeEwEwYDVR0lBAwwCgYIKwYBBQUHAwEwCwYDVR0PBAQDAgWgMA0GCSqGSIb3DQEBCwUAA4ICAQC9WZuYgQedSuOc5TOUSrRigMw4/+wuC5EtZBfvdl4HT/8vzMW/oUlIP4YCvA0XKyBaCJ2iX+ZCDKoPfiYXiaSiH+HxAPV6J79vvouxKrWg2XV6ShFtPLP+0gPdGq3x9R3+kJbmAm8w+FOdlWqAfJrLvpzMGNeDU14YGXiZ9bVzmIQbwrBA+c/F4tlK/DV07dsNExihqFoibnqDiVNTGombaU2dDup2gwKdL81ua8EIcGNExHe82kjF4zwfadHk3bQVvbfdAwxcDy4xBjs3L4raPLU3yenSzr/OEur1+jfOxnQSmEcMXKXgrAQ9U55gwjcOFKrgOxEdek/Sk1VfOjvS+nuM4eyEruFMfaZHzoQiuw4IqgGc45ohFH0UUyjYcuFxxDSU9lMCv8qdHKm+wnPRb0l9l5vXsCBDuhAGYD6ss+Ga+aDY6f/qXZuUCEUOH3QUNbbCUlviSz6+GiRnt1kA9N2Qachl+2yBfaqUqr8h7Z2gsx5LcIf5kYNsqJ0GavXTVyWh7PYiKX4bs354ZQLUwwa/cG++2+wNWP+HtBhVxMRNTdVhSm38AknZlD+PTAsWGu9GyLmhti2EnVwGybSD2Dxmhxk3IPCkhKAK+pl0eWYGZWG3tJ9mZ7SowcXLWDFAk0lRJnKGFMTggrWjV8GYpw5bq23VmIqqDLgkNzuoog==

运行异常Failed to load module “canberra-gtk-module”
sudo apt-get install libcanberra-gtk-module
```

- centerOs install git
```
yum info git
yum install git

```

- win
```
Windows 10教育版－YNMGQ-8RYV3-4PGQ3-C8XTP-7CFBY
Windows 10教育版－84NGF-MHBT6-FXBX8-QWJK7-DRR8H
Windows 10 Education - NW6C2-QMPVW-D7KKK-3GKT6-VCFB2
Windows 10 Education N - 2WH4N-8QGBV-H22JP-CT43Q-MDWWJ
Windows 10 Education 永久激活-HSGFY-8SHBC-AZKIR-7SHEN-0SYRH
Windows 10 Education 永久激活-QPEO4-NVBS7-AHF3K-AJD5H-AJFK8
 
Windows 10 Education 永久激活零售光盘－XCBKW-MHNQ9-VTKMW-99T6Y-6VJWB
Windows 10 Education 永久激活零售光盘－XMYDD-HPNTH-TJKCH-8GY8T-9KXRM
```

- jd2chm
```
https://www.burgaud.com/jd2chm.html
问题：HTML Help Workshop does not seem to be installed 系统找不到指定的文件。
解决：https://docs.microsoft.com/zh-cn/previous-versions/windows/desktop/htmlhelp/microsoft-html-help-downloads 下载即可
```