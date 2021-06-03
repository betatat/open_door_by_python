## 说明
智慧南山微信小程序，南山区视频门禁服务小程序可一键开启门禁，但开启小程序过程总是特别慢，操作比较繁琐。

既然都是发起请求，那么使用python的requests模块直接开门更加方便，为此编写了这个简单的项目。

项目中包含敏感信息的内容已替换，如`API域名`、`微信OpenID`、`Token`、`RequestData`等，这些可通过抓包获得属于自己的。

本项目只适用于特定小程序。

## 实现过程
### 登录微信抓包
工具：抓包软件使用`fiddler4`，微信版本`3.1.0.72`

开启Fiddler的HTTPS捕获以及解密功能。

清空此前捕获的流量，以便于快速抓取到小程序发起的请求。

使用小程序发起“开门”请求

随后找到捕获的小程序流量，选择Filter Now，选择show only process = XXXX后，仅显示对应小程序的请求。

至此抓包过程结束，将fiddler的内容保存（file→save→all session）。

替换程序内`抓包获取`的对应内容。

### 分析请求内容
通过分析发现，进入小程序后，会立即获得房主的个人信息（姓名、位置、houseHostId），并请求账号的token

而开门只需要账号`token`、`微信OpenID`、`houseHostId`与开门`secret`，发送一个post请求即可。

发现获取`token`、`secret`、`opendoor`三步的请求头略有差异，构造每个步骤的请求头。

获取对应的token后，构建URL+token，将`微信OpenID`、`houseHostId`、`secret`包含在requests的请求体中，以json发送即可开门。

### 运行
使用`vscode`直接运行，或在py文件的同目录创建一个批处理
```batch
@echo off
python open_the_door.py
pause
```

## 手机应用
安卓手机可使用`pydroid`运行

苹果手机可编写`快捷指令`实现相同效果
