中国电信物联网开放平台API（北向）对接库
=======================

[![GitHub license](https://img.shields.io/github/license/zeevin/libiocm.svg)](https://github.com/zeevin/libiocm/blob/master/LICENSE)
[![GitHub forks](https://img.shields.io/github/forks/zeevin/libiocm.svg)](https://github.com/zeevin/libiocm/network)
[![GitHub stars](https://img.shields.io/github/stars/zeevin/libiocm.svg)](https://github.com/zeevin/libiocm/stargazers)

Libiocm 实现了对中国电信物联网开发平台（北向）API的对接。

### 此sdk还在开发过程中，当前仅仅推荐给大家测试用。预计最晚2018年5月20号完成全部功能，在此期间请勿直接用于正式环境！，有兴趣的也可以加QQ群交流：群号:577640752

截止到2018年5月16日，已完成对接的接口列表：
- 1.2.1应用安全接入：应用安全接入：Auth（鉴权）、Refresh Token（刷新Token）、注销
- 1.2.2设备管理：注册直连设备、查询设备激活状态、删除直连设备、修改设备信息、刷新设备密钥
- 1.2.3数据采集：按条件批量查询设备信息列表、查询单个设备信息


2018年5月17日，新对接的接口列表：
- 1.2.3数据采集：Application 订阅平台数据、查询设备历史数据、查询设备能力
- 1.2.4信令传送：创建设备命令V4

安装：
```bash
composer require  zeevin/libiocm "@dev"
```

主要完成如下内容：

- 1、使用JMS包装请求参数。以定义类变量的方式设置请求参数，便于参数的设置和参数提示。
- 2、使用JMS包装返回的json数据，默认会把请求数据映射为对应的RequestAttribute类，便于进一步使用。
- 3、使用doctrine/cache 缓存ouath token结果。目前适配了memcached、Redis、file三类。

注意事项：
- 1、Guzzle 库只支持pem格式的证书，因此需要将默认的p12格式证书转换成pem格式，比如：
```bash
openssl pkcs12 -in outgoing.CertwithKey.pkcs12 -out key.pem -nodes -clcerts
```
目前测试平台的证书密码是：IoM@1234，如果后期电信有更新需要同步更新。

- 2、电信编写的api文档看起来并不十分完善，有些返回信息是从头信息中查看，比如1.2.2.4 删除直连设备接口，
此接口在删除成功时返回结果需要查看头信息的statusCode和reasonPhrase内容，body中并没有json信息，为解决这种问题，
我在所有的请求结果中都加入了"statusCode"和"reasonPhrase"属性。

- 3、接口调用方式请查看tests文件夹下的示例。