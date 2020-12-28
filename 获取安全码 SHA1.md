获取安全码 SHA1
-------------

### 解决方案:

> 再控制台中输入以下命令再拼接上你的签名路径

```
keytool -exportcert -list -v -keystore 你的签名路径.jks

```

### 拓展阅读

[推荐文章](https://www.jianshu.com/p/dcfca6041154)