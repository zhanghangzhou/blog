Tomcat SSL Configuration
---
# 1 生成证书请求
``` bash
# 生成密钥对
keytool -genkey -keyalg RSA -keysize 1024 -dname "CN=192.168.131.164,OU=BJCA,O=BJCA,L=Beijing,ST=Beijing,C=CN" -alias server -keypass 123456 -keystore server.jks -storepass 123456 -validity 365
# 导出证书请求
keytool -certreq -alias server -sigalg "SHA1withRSA" -file server.pem -keypass 123456 -keystore server.jks -storepass 123456
```
> CN应为域名。

# 2 申请证书
使用证书请求server.pem向第三方CA申请SSL证书。

# 3 导入证书
## 3.1 导入CA证书
> 准备根证书root.cer和二级CA证书ca.cer。

``` bash
# 导入根证书
keytool -import -alias rootca1 -keystore server.jks -trustcacerts -storepass 123456 -file root.cer
# 导入二级CA证书
keytool -import -alias ca1 -keystore server.jks -trustcacerts -storepass 123456 -file ca.cer
```
## 3.2 导入服务器证书（回复证书）
``` bash
keytool -import -alias server -keystore server.jks -trustcacerts -storepass 123456 -file server.cer
```

# 4 配置https
> JKS相对根目录在Tomcat主目录下。

## 4.1 单向https配置
配置server.xml文件
``` xml
<Connector port="443" protocol="org.apache.coyote.http11.Http11NioProtocol"
   maxThreads="500" SSLEnabled="true" scheme="https" secure="true"
   clientAuth="false" sslProtocol="TLS" sslEnabledProtocols="TLSv1,TLSv1.1,TLSv1.2"
   keystoreFile="bsam.jks" keystorePass="changeit" keystoreType="JKS"
   ciphers="TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA,TLS_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_256_CBC_SHA256,TLS_RSA_WITH_AES_256_CBC_SHA"
   connectionTimeout="5000" />
```
> sslEnabledProtocols：  
> sslProtocol：

## 4.2 双向https配置
配置server.xml文件
``` xml
<Connector
		port="8443" minSpareThreads="5" maxSpareThreads="75"
		enableLookups="true" disableUploadTimeout="true"
		acceptCount="100" maxThreads="200"
		scheme="https" secure="true" SSLEnabled="true"
		clientAuth="true" sslProtocol="TLS" sslEnabledProtocols="TLSv1,TLSv1.1,TLSv1.2"
		keystoreFile="server.jks" keystorePass="123456"
    ciphers="TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA,TLS_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_256_CBC_SHA256,TLS_RSA_WITH_AES_256_CBC_SHA"
		truststoreFile="server.jks" truststorePass="123456"/>
```

> setProtocol相关参数含义：  
"TLS" will enable SSLv3 and TLSv1  
"TLSv1.2" will enable SSLv3, TLSv1, TLSv1.1 and TLS v1.2  
"TLSv1.1" will enable SSLv3, TLSv1, and TLSv1.1  
"TLSv1" will enable SSLv3 and TLSv1  
"SSL" will enable SSLv3 and TLSv1  
"SSLv3" will enable SSLv3 and TLSv1  
"SSLv2" won't work

> sslEnabledProtocols在setProtocol的基础上详细协议配置
sslEnabledProtocols="TLSv1,TLSv1.1,TLSv1.2"

 > 为了安全及适用于最新版的浏览器，需要配置支持的加密算法
``` ciphers="TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA,TLS_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_256_CBC_SHA256,TLS_RSA_WITH_AES_256_CBC_SHA"
```

# 5 配置http自动跳转到https
## 5.1 配置server.xml文件
``` xml
<!-- 80的转发端口改成443 -->
<Connector port="80" protocol="HTTP/1.1"
   connectionTimeout="20000"
   redirectPort="443" />
```

## 5.2 配置tomcat/conf/web.xml文件
> 在web-app节点中（如在</web-app>前）添加如下配置。

``` xml
<login-config>
  <auth-method>CLIENT-CERT</auth-method>
  <realm-name>Client Cert Users-only Area</realm-name>
</login-config>
<security-constraint>
  <web-resource-collection >
    <web-resource-name >SSL</web-resource-name>
    <url-pattern>/*</url-pattern>
  </web-resource-collection>
  <user-data-constraint>
    <transport-guarantee>CONFIDENTIAL</transport-guarantee>
  </user-data-constraint>
</security-constraint>
<!--</web-app>-->
```
