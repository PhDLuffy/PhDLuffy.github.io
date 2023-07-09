---
layout: post
title: 一环扣一环的问题链条
subtitle: frp增加https反代使homeassistant拥有ssl能力接收smartthing传入数据
date: 2023-07-10
author: PhDLuffy
header-img: img/Blog/02.jpg
music-id: 29802490
catalog: true
tags:
    - 奇技淫巧

---

## 问题链条：

- HA的官方cloud账号到期，Smartthing的webhook需要改用自己的https网址
- Oracle服务器的80和443端口被pan.phdluffy.com网盘网站给占用了，所以使用frp的81和444端口来反代https网址
- frp的服务器端升级到[0.51.0](https://github.com/MvsCode/frps-onekey)，客户端升级到[0.51.0](https://registry.hub.docker.com/r/snowdreamtech/frpc/tags)
- frp中添加不同服务的二级域名ss证书，证书去[zerossl](https://zerossl.com/)下载
- 证书涉及一个非常重要的Certificate Chain Complete问题，在[ssl-checker](https://www.geocerts.com/ssl-checker)验证，需要将certificate.crt证书和ca_bundle.crt证书进行合并，这样才会被Smartthing所接受
- HA的comfiguration.yaml添加`http`字段，这样可以用过frp反代接入，同时添加external_url和internal_url字段，这样可以外网https，内网http，以免和node-red冲突
- HA中输入Smartthing的token，接入三星空气检测即可

## 分步解决

### frp

#### frps服务端升级[0.51.0](https://github.com/MvsCode/frps-onekey)，

分三行命令，

1. 下载`./install-frps.sh`文件到当前目录
2. 给与`./install-frps.sh`运行权限
3. 运行`./install-frps.sh`安装frps

```zsh
wget https://raw.githubusercontent.com/mvscode/frps-onekey/master/install-frps.sh -O ./install-frps.sh
chmod 700 ./install-frps.sh
./install-frps.sh install
```

安装过程中注意的点，

- vhost_http_port = 81
- vhost_https_port = 444

分别选择81和444是因为同一台服务器上有宝塔面板搭建的pan.phdluffy.com网盘网站，

Ngnix已经占用了80和443端口，

同时Xui应用会使用`网址/二级目录`进行管理和生成FQ代理链接

subdomain选择: phdluffy.com

在frpc的ini配置中会在此网址前添加二级网址

#### frpc客户端升级到[0.51.0](https://registry.hub.docker.com/r/snowdreamtech/frpc/tags)

Termius登录DSM，直接`docker pull snowdreamtech/frpc:latest`拉取最新的镜像

编辑frpc.ini文件

```json
[HA_http]
type = http
local_ip = [DSM的ip]
local_port = [HA本地端口]
subdomain = ha

[HA_https]
type = https
local_ip = [DSM的ip]
local_port = [HA本地端口]
subdomain = ha
plugin = https2http
plugin_local_addr = 127.0.0.1:[HA本地端口]
plugin_host_header_rewrite = 127.0.0.1
plugin_header_X-From-Where = frp
plugin_crt_path = /etc/frp/ssl/ha/certificate.crt
plugin_key_path = /etc/frp/ssl/ha/private.key
```

注意的点：

- 证书的路径是docker容器内部的绝对路径，可以将此路径暴露到容器外进行管理，不同的服务和端口可以使用不同的ssl证书

- 非常重要：crt证书必须是certificate.crt证书和ca_bundle.crt证书进行合并之后的证书

  > [Unable to Add SmartThings Integration - Third party integrations - Home Assistant Community](https://community.home-assistant.io/t/unable-to-add-smartthings-integration/235070)
  >
  > 合并方法也非常简单，将两部分证书内容拼接在一起就可以了，certificate code在上面，紧接着intermediate certificate code即可
  >
  > ![合并证书方法](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202307100028054.png)

cd到frp文件夹，`docker-compose up -d`即可重新加载容器

同样的方法可以加载https到不同的服务端口上并启用不同的二级域名，只需要复制同样的操作即可，

统一使用444的https端口，如果443没被占用就可以不输入端口号就能用了。

### zerossl

zerossl申请新邮箱申请新的90天ssl

二级域名，比如ha.phdluffy.com，需要手动到[cloudflare](https://www.cloudflare.com/)进行CNAME验证，验证完毕之后，下载证书

证书解压缩，有三个文件，一定记得合并certificate.crt证书和ca_bundle.crt证书

### HomeAssistant

HomeAssistant的comfiguration.yaml文件，

添加：

```json
  external_url: "https://外网地址+端口"
  internal_url: "http://内容地址+端口"
```

外网使用https访问，内网使用http访问，避免node-red出现关联问题

添加:

```json
http:
  use_x_forwarded_for: true
  trusted_proxies:
    - [ip地址]
    - [ip地址]
    - [ip地址]
  ip_ban_enabled: true
  login_attempts_threshold: 5
```

trusted_proxies:添加信任的ip地址，例如本地地址，127.0.0.1，还有远程服务器地址

重启HomeAssistant容器

尝试从外网访问https的HA是否成功

### Smartthing

进入HA，配置，添加集成，Smartthing，点击确定，添加Smartthing的token即可。



以上。









