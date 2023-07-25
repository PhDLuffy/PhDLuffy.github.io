---
layout: post
title: 自动申请泛域名证书
subtitle: zerossl通过came.sh申请泛域名证书用于各个服务
date: 2023-07-10
author: PhDLuffy
header-img: img/Blog/06.jpg
music-id: 29802490
catalog: true
tags:
    - 奇技淫巧

---

## Docker

- docker文件夹下新建`acme.sh`文件夹

- 创建data文件夹，内部acme.sh/data/account.conf

  ```json
  export CF_Key="Cloudflare的API-KEY"
  export CF_Email="Cloudflare的账号邮箱"
  AUTO_UPGRADE='1'
  ```

- 创建docker-compose文件，acme.sh/docker-compose.yml

  ```json
  version: "3"
  services:
    acme.sh:
      image: neilpang/acme.sh
      container_name: acme.sh
      restart: always
      command: daemon
      volumes:
        - /volume1/docker/acme.sh/data:/acme.sh
      network_mode: host
  ```

- Termius连接DSM，`docker-compose up -d`运行容器

## 申请泛域名证书

[群晖DSM7.1系统自动SSL证书更新](https://blog.pigcyan.com/archives/%E7%BE%A4%E6%99%96dsm71%E7%B3%BB%E7%BB%9F%E8%87%AA%E5%8A%A8ssl%E8%AF%81%E4%B9%A6%E6%9B%B4%E6%96%B0)参考此文1.2

- Termius连接DSM

- 进入docker容器内命令行

  `docker exec -it acme.sh sh`

- 更新acme.sh程序，（可选）

  `acme.sh --upgrade --auto-upgrade`

- 申请泛域名证书，默认zerossl，第一次运行可能需要根据提示注册zerossl邮箱

  `acme.sh --issue --dns dns_cf -d phdluffy.com -d *.phdluffy.com`

  > 此例中dns_cf代表Cloudflare解析，如果有其他dns服务商需求可以自己修改

- 运行成功之后，证书会下载到/data/域名_ecc

各证书含义：

[根证书、服务器证书、用户证书的区别 - 小李y - 博客园](https://www.cnblogs.com/xiaoli-ya/p/16144791.html)---参考此文

- ca.cer证书包含中间证书和根证书
- 域名.cer证书是服务器证书
- fullchain.cer证书是上面两个证书的结合
- 域名.key是证书秘钥

上传到X-UI的证书选择第2个域名.cer证书和private.key即可
用于home assistant的外部链接frp反代的ha的ssl文件夹中选择fullchain.cer和域名.key

![image-20230710124629262](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202307101246908.png)



![image-20230710124919053](https://fastly.jsdelivr.net/gh/PhDLuffy/PicGo@master/img/202307101249083.png)
