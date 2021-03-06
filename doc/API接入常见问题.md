# 1 音箱无法访问腾讯云叮当后台域名
## 1.1 问题背景
- AVS兼容API接入方式
## 1.2 表现形式
- 该机器之前运行良好，重新刷一个ROM版本后，无法访问腾讯云叮当后台域名 “https://tvs.html5.qq.com”。
## 1.3 问题原因
- 刷机之后，机器的系统时间变成了**“2018.1.1”**。
- “https://tvs.html5.qq.com“ 使用了“\*.html5.qq.com”证书。该证书的有效期为“2018.3.8 - 2019.3.9”。
- 终端会验证证书的有效性，**终端本地时间戳不在证书的有效期内**，因此在SSL握手过程中失败。
## 1.4 解决方法
- 修改终端的时钟，调整为当前时间。

# 2 访问叮当域名证书过期问题
## 2.1 问题背景
- 云端API-基础API接入方式。
- 后台对后台的方式。
## 2.2 表现形式
- 2018.x.xx日厂商后台服务器访问域名：https://aiwx.html5.qq.com/api/v1/richanswer， 报SSL证书验证失效。
- 厂商后台提示证书有效期至：2018.x.xx 7:59:59。
- 厂商后台访问其它公司域名正常。
- 其它厂家访问叮当域名正常。
- 在腾讯云使用demo脚本访问叮当域名，也能正常访问。
- 在PC浏览器上访问叮当域名，查看证书是在有效期内的。
## 2.3 问题原因
- 厂商后台使用的TLSV1协议，该协议不支持SNI协议。
- 腾讯接入层后台对不支持SNI协议的终端，会下发默认证书。
- 默认证书刚好该时间节点过期。
### 2.4 解决方法
- 快速修复：厂商后台将域名从https协议修改为http，可快速修复。
- 腾讯接入层更新证书后，有效期延长，恢复对厂商后台的https能力。
- 厂商后台升级TLSV1到TLSV1.2，避免使用默认证书。