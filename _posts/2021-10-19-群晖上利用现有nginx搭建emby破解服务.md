---
layout: post
title: 群晖上利用现有nginx搭建emby破解服务
author: lengyue524
comments: true
categories: [emby]
tags: [群晖,emby]
description: 群晖上利用现有nginx搭建emby破解服务
---

1. 创建证书，网上搜索教程即可



2. 添加下面的文件

/usr/local/etc/nginx/sites-enabled/mb3admin.com.conf

注意ssl_certificate，ssl_certificate_key修改为自己证书的路径

```json
server {
    listen 80;
    listen 443 ssl;
    server_name mb3admin.com;

    ssl_certificate /volume2/homes/lengyue524/mb3admin.com/server.crt;
    ssl_certificate_key /volume2/homes/lengyue524/mb3admin.com/server.key;
    add_header Access-Control-Allow-Origin *;
    add_header Access-Control-Allow-Headers *;
    add_header Access-Control-Allow-Method *;
    add_header Access-Control-Allow-Credentials true;
    location /admin/service/registration/validateDevice {
        default_type application/json;
        return 200 '{"cacheExpirationDays": 3650,"message": "Device Valid","resultCode": "GOOD"}';
    }

    location /admin/service/registration/validate {
        default_type application/json;
        return 200 '{"featId": "","registered": true,"expDate": "2099-01-01","key": ""}';
    }

    location /admin/service/registration/getStatus {
        default_type application/json;
        return 200 '{"planType": "Lifetime", "deviceStatus": 0, "subscriptions": []}';
    }

    location /admin/service/appstore/register {
        default_type application/json;
        return 200 '{"featId": "","registered": true,"expDate": "2099-01-01","key": ""}';
    }

    location /emby/Plugins/SecurityInfo {
        default_type application/json;
        return 200 '{"SupporterKey": "", "IsMBSupporter": true}';
    }
}
```



3. 本机添加host

```shell
nginx服务ip mb3admin.com
```

4. 让浏览器信任不安全连接
浏览器打开”https://mb3admin.com/admin/service/registration/getStatus“ 会显示不受信任的连接，选择更多设置，信任。
