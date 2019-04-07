---
title: NodeRed-Modbus相关
date: 2019-04-05 18:31:53
categories: 数据接入
tags: 
- node-red
- modbus
---

## 目录
<!-- toc -->



## 安装

modbus采集需要安装第三方采集节点，以 node-red-contrib-modbus` 为例

在node-red运行目录下执行：

```shell
npm install node-red-contrib-modbus
```



## Modbus采集操作步骤

1. #### 配置Modbus Slave 模拟器

![image-20190405162528786](https://ws3.sinaimg.cn/large/006tNc79ly1g1ru4k7sdwj30n80p676i.jpg)

![image-20190405162713230](https://ws2.sinaimg.cn/large/006tNc79ly1g1ru6dhks6j30k20la76c.jpg)

2. #### 添加轮询节点

![image-20190405162846822](https://ws2.sinaimg.cn/large/006tNc79ly1g1ru7zx7g4j30ui0ko0tv.jpg)

3. #### 配置读取方式

![image-20190405163123119](https://ws4.sinaimg.cn/large/006tNc79ly1g1ruaqarsvj30vf0u0tb6.jpg)

4. #### 添加模拟器地址

![image-20190405163023419](https://ws2.sinaimg.cn/large/006tNc79ly1g1ru9oodupj30s613g76n.jpg)

5. #### 添加debug输出，用于调试

![image-20190405163336711](https://ws3.sinaimg.cn/large/006tNc79ly1g1rud01mrnj30tq04kaa8.jpg)

6. #### 采集数据示例

![image-20190405163441799](https://ws3.sinaimg.cn/large/006tNc79ly1g1rue6elgpj31i60u0wit.jpg)
