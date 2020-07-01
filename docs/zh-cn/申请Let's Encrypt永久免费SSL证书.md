### 申请Let's Encrypt永久免费SSL证书

#### Let's Encrypt简介

[Let's Encrypt](https://link.jianshu.com?t=https%3A%2F%2Fletsencrypt.org%2F)作为一个公共且免费SSL的项目逐渐被广大用户传播和使用，是由Mozilla、Cisco、Akamai、IdenTrust、EFF等组织人员发起，主要的目的也是为了推进网站从HTTP向HTTPS过度的进程，目前已经有越来越多的商家加入和赞助支持。

[Let's Encrypt免费SSL](https://link.jianshu.com?t=https%3A%2F%2Fletsencrypt.org%2F)证书的出现，也会对传统提供付费SSL证书服务的商家有不小的打击。到目前为止，Let's Encrypt获得IdenTrust交叉签名，这就是说可以应用且支持包括FireFox、Chrome在内的主流浏览器的兼容和支持，虽然目前是公测阶段，但是也有不少的用户在自有网站项目中正式使用起来。

#### 步骤如下：

**第一、安装Let's Encrypt前的准备工作**

```java
安装letsencrypt
yum install git python
 
git clone https://github.com/letsencrypt/letsencrypt

如若在执行git clone命令时出现以下错误：

Cloning into 'letsencrypt'...
fatal: unable to access 'https://github.com/letsencrypt/letsencrypt/': Peer reports incompatible or unsupported protocol version.
须执行以下命令升级nss、curl、libcurl：

yum update -y nss curl libcurl

```

**第二、获取Let's Encrypt免费SSL证书**

```java
#获取letsencrypt
git clone https://github.com/letsencrypt/letsencrypt
#进入letsencrypt目录
cd letsencrypt
```

```java
#未安装nginx或apache等web服务器
./letsencrypt-auto certonly --standalone --email wenbinwumei@qq.com -d free4sydney.com -d www.free4sydney.com
#安装nginx或apache等web服务器
# ./letsencrypt-auto certonly --standalone --email wenbinwumei@qq.com -d free4sydney.com --webroot-path=/var/www/free4sydney.com
wenbinwumei@qq.com为你的邮箱，
free4sydney.com为待签发的域名，
/var/www/free4sydney.com为web服务器中定义的虚拟主机目录.
```

**第三、Let's Encrypt免费SSL证书获取与应用**

在完成Let's Encrypt证书的生成之后，我们会在"/etc/letsencrypt/live/zhaoheqiang.me/"域名目录下有4个文件就是生成的密钥证书文件。

cert.pem  - Apache服务器端证书
chain.pem  - Apache根证书和中继证书
fullchain.pem  - Nginx所需要ssl_certificate文件
privkey.pem - 安全证书KEY文件

如果我们使用的Nginx环境，那就需要用到fullchain.pem和privkey.pem两个证书文件，在部署Nginx的时候需要用到。在Nginx环境中，只要将对应的ssl_certificate和ssl_certificate_key路径设置成我们生成的2个文件就可以。

```java
#打开linux配置文件，找到HTTPS 443端口配置的server
 ssl_certificate /etc/letsencrypt/live/zhaoheqiang.me/fullchain.pem;
 ssl_certificate_key /etc/letsencrypt/live/zhaoheqiang.me/privkey.pem;
```

**第四、解决Let's Encrypt免费SSL证书有效期问题**

Let's Encrypt证书是有效期90天的，需要我们自己手工更新续期才可以。
 *命令如下：*

```java
 ./letsencrypt-auto certonly --renew-by-default --email wenbinwumei@163.com -d free4sydney.com -d www.free4sydney.com
```

这样我们在90天内再去执行一次就可以解决续期问题，这样又可以继续使用90天。如果我们怕忘记的话也可以利用linux  crontab定时执行更新任务

```
* * * 2 * /letsencrypt/renewTask.sh
```



**第五、HTTPS单向认证服务器构建(tomcat为服务器)**

我个人用的是tomcat-8.0.38 ，tomcat-8.5.3支持直接部署.pem文件

```java
// 在tomcat平级建立文件夹 LetsEncrypt
```

```java
将/etc/letsencrypt/live/china-car.xyz中的fullchain.pem 为证书  privatkey.pem 为密钥复制到该文件夹下
cp /etc/letsencrypt/live/china-ecar.xyz/fullchain.pem  fullchain.pem
cp /etc/letsencrypt/live/china-ecar.xyz/privkey.pem  privkey.pem
```

```java
生成.p12文件
openssl pkcs12 -export -in fullchain.pem -inkey privkey.pem -out keystore.p12 -name tomcat
这里会要求设置密码，及下面代码中的'yourPKCS12pass'
```

```java
.jks证书
keytool -importkeystore -deststorepass 'yourJKSpass' -destkeypass 'yourKeyPass' -destkeystore MyDSKeyStore.jks -srckeystore keystore.p12 -srcstoretype PKCS12 -srcstorepass 'yourPKCS12pass' -alias tomcat
其中yourPKCS12pass 是上一步中设置的ssl证书密码，这里的yourKeyPass是要设置的keystore密码，可以与yourPKCS
```

修改tomcat conf/server.xml文件如下

重启服务器