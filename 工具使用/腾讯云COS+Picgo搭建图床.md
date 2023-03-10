---
title: 腾讯云COS+Picgo搭建图床
date: 2023-01-12 23:58:52
comments: true
toc: true
tags: [图床]
categories: [工具使用]
---

## 写在前面

jsDelivr凉了，因此一直在寻找新的个站图片存储方案，最终还是觉得腾讯云的对象存储服务比较合适，在此分享折腾的过程。

<!-- more -->

先来介绍两个概念：

* **对象存储**

> 对象存储（Cloud Object Storage，COS）是腾讯云提供的一种存储海量文件的分布式存储服务，用户可通过网络随时存储和查看数据。腾讯云 COS 使所有用户都能使用具备高扩展性、低成本、可靠和安全的数据存储服务。
> COS 通过控制台、API、SDK 和工具等多样化方式简单、快速地接入，实现了海量数据存储和管理。通过 COS 可以进行任意格式文件的上传、下载和管理。腾讯云提供了直观的 Web 管理界面，同时遍布全国范围的 CDN 节点可以对文件下载进行加速。

* **存储桶**

> 存储桶（Bucket）是对象的载体，可理解为存放对象的“容器”，且该“容器”无容量上限。对象以扁平化结构存放在存储桶中，无文件夹和目录的概念，用户可选择将对象存放到单个或多个存储桶中。

---



## 购买对象存储(cos)资源包

购买链接：https://curl.qcloud.com/CcQyuzkZ

![image-20220518124256171](https://cdn.jsdelivr.net/gh/sxfinn/CDN/img/202212021514174.png)

## 创建存储桶列表

> 这里是因为我用做来做个人图床，所以考虑到自己的一个用量，10G完全满足我了。其中流量中的外网下行流量包(图片访问一次就会产生费用)，和CDN回源流量包(CDN节点向 COS 获取数据所产生的CDN回源流量)，其实也要购买的，但是我是小站，用不了那么多，直接选择按量计费，不选择购买这两个的资源包，余额准备几块钱就好了。

* 来到腾讯云[对象存储](https://cloud.tencent.com/product/cos?from=10680)控制台，创建存储

![image-20220518122221501](https://cdn.jsdelivr.net/gh/sxfinn/CDN/img/202212021514925.png)

![image-20220518122519936](https://cdn.jsdelivr.net/gh/sxfinn/CDN/img/202212021514032.png)

* 高级配置可以不去管它

![image-20220518122547786](https://cdn.jsdelivr.net/gh/sxfinn/CDN/img/202212021515045.png)

- 访问权限选择**公有读私有写**，否则图片无法读取，其他的根据自己往下填写就可以。 地域建议离你所在的位置越近越好。 
- 腾讯云头像–>[访问管理](https://cloud.tencent.com/product/cam?from=10680)–> [API密钥管理](https://cloud.tencent.com/product/ssm?from=10680)，创建密钥，就会生成 **APPID、SecretId和SecretKey**

![image-20220518122910316](https://cdn.jsdelivr.net/gh/sxfinn/CDN/img/202212021515196.png)

---



## 配置Picgo

* 打开 **PicGO**；
* 进入**腾讯云COS设置**
* 填写好前面三个信息后，**存储空间名和区域**的信息在刚刚创建对象存储控制台那里，这里注意的是，**COS版本选择 v5** ；
* 设定存储空间名是**存储桶的名字**；
* 存储区域通常是ap-刚刚选择的地区名
* 指定存储路径选择img/，暂时不需要填写自定义域名；

​		记得设置为默认图床，否则picgo不会默认上传到COS；

![image-20220518123144471](https://cdn.jsdelivr.net/gh/sxfinn/CDN/img/202212021515357.png)

![image-20220518123240046](https://cdn.jsdelivr.net/gh/sxfinn/CDN/img/202212021515396.png)



## 配置Typora

* 勾选本地和网络位置的图片使用上传规则，如图；

![image-20220519105752449](https://cdn.jsdelivr.net/gh/sxfinn/CDN/img/202212021515964.png)

验证图片上传选项成功即可。

到此，就可以直接使用Picgo上传图片了，这时图片的链接是 `https://sxnico-imgaes-1310265079.cos.ap-chengdu.myqcloud.com`（对于我而言，取决于你的存储桶名称）

COS也支持公网直接访问，但是流量费太贵，直接访问不划算，况且腾讯云还每个月会赠送10G的免费CDN流量，完全可以利用起来。

对于访问量少的站点来说完全足够了，不过这还不够好，最近注意到腾讯云COS+CDN的搭配既能不占用服务器的宽带，又能快速上传下载文件，而且以我目前的使用量来说基本上不花钱，这里分享下我的折腾过程，在我的另一篇文章中。

---

参考文章：

1. [腾讯云COS对象存储+PicGo搭建图床教程 - 腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/1834573)
2. [腾讯云COS搭建图床-it小离](https://www.itxiaoli.cn/archives/pic.html)
