### wfuzz

最基本的

```
wfuzz -w dict url/FUZZ
```



如果字典很大，我们需要过滤 来获得我们想要的信息。

隐藏hc/hl/hw/hh(code/lines/words/chars)

```
隐藏404 403状态码
wfuzz -w /usr/share/wfuzz/wordlist/general/common.txt --hc 404,403 
```

### ffuf

```
ffuf -c -w /home/day0xy/Documents/penetration/web/Blasting-dictionary/fuzzDicts/paramDict/AllParam.txt  -u 'http://192.168.1.103/blog-post/archives/randylogs.php?FUZZ=/etc/passwd' -fs 0

这里末尾是过滤了Size 0
```

