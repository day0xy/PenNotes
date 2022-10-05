### 文件包含 



#### php伪协议

```
file://
http://
ftp://
php://
zip://
data://
.
.

```

##### php://输入输出流



##### php://filter

需要开启allow_url_fopen,不需要开启allow_url_include





###### 转换过滤器

```
?filename=php://filter/convert.x/resource=flag.php
x替换为编码
x=
{
base64-encode
base64-decode
quoted-printable-encode
quoted-printable-decode

iconv.(input encoding).(output encoding)
iconv.(input encoding)/(output encoding)
iconv{
UCS-4*
UCS-4BE
UCS-4LE*
UCS-2
UCS-2BE
UCS-2LE
UTF-32*
UTF-32BE*
UTF-32LE*
UTF-16*
UTF-16BE*
UTF-16LE*
UTF-7
UTF7-IMAP
UTF-8*
ASCII*}
}



}
```

