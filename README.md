# AESJniEncrypt
- [x] ndk实现AES加密
- [x] 使用JniOnload 隐藏c函数
- [x] 再做一层防止被二次打包的签名校验
- [x] key存在符号表中，同时隐藏字符表
- [ ] 使用OLLVM混淆C的代码

集成到自己的项目中，key 请自行修改。
```
static const uint8_t AES_KEY[]="1234567890abcdef";
```


## keystore文件 校验

1.生成
```
//再当前目录下
$ mkdir  keystore
$ cd keystore/
$ keytool -genkey -alias client1 -keypass 123456 -keyalg RSA -keysize 1024 -validity 365 -storetype PKCS12 -keystore ./androidyuan.keystore
输入密钥库口令:weizongwei
再次输入新口令:weizongwei
警告: PKCS12 密钥库不支持其他存储和密钥口令。正在忽略用户指定的-keypass值。
您的名字与姓氏是什么?
  [Unknown]:  weizongwei
您的组织单位名称是什么?
  [Unknown]:  androidyuan.com
您的组织名称是什么?
  [Unknown]:  androidyuan.com
您所在的城市或区域名称是什么?
  [Unknown]:  shanghai
您所在的省/市/自治区名称是什么?
  [Unknown]:  shanghai
该单位的双字母国家/地区代码是什么?
  [Unknown]:  cn
CN=weizongwei, OU=androidyuan.com, O=androidyuan.com, L=shanghai, ST=shanghai, C=cn是否正确?
  [否]:  y

//测试 keystore口令是否正确
$ keytool -exportcert -alias androiddebugkey -keystore   "androidyuan.keystore" | openssl sha1 -binary | openssl base64
  输入密钥库口令:  weizongwei
  8GUZG0hBFvUZ1I4kSq/3vowhE7Y=


```

## 2.取得当前打包的keystore的hash值

    目前似乎没有好的办法，我只能用java取，取出之后打log，然后写入到C文件中，重新build项目。
    
  集成到自己项目中请先修改keystore hashcode和包名，防止反编译时拿到so文件，进行二次打包使用。
## 鸣谢

Base64 算法 来自：https://github.com/willemt/pearldb

AES128 算法 来自：https://github.com/kokke/tiny-AES128-C
