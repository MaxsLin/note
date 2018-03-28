# P12证书 转成 PEM 证书

---

![image](https://raw.githubusercontent.com/MaxsLin/note/master/image/QQ20180328-165700%402x.png)


> 把cert 导出 执行

```
    openssl pkcs12 -clcerts -nokeys -out apns_dev_cert.pem -in apns_dev_cert.p12
```

> 把 Key 导出 执行

```
openssl pkcs12 -nocerts -out apns_dev_key.pem -in apns_dev_key.p12
//要求输入一个密码，输入123456.（此处为导出p12的保护密码）
```

> 将apns_dev_cer.pem和apns_dev_key.pem文件合成为apns_dev.pem文件
```
cat apns_dev_cer.pem apns_dev_key.pem > apns_dev.pem
```

> 验证
```
openssl s_client -connectgateway.sandbox.push.apple.com:2195 -cert apns_dev_cert.pem -key apns_dev_key.pem

// 结果
Key-Arg   : None

Start Time: 1467854873

Timeout   : 300 (sec)

Verify return code: 0 (ok)
```
