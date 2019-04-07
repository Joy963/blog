---
title: RedGateway使用说明
date: 2019-04-05 18:55:57
categories: 数据接入
tags: 
- GoLang
- RedGateway
---

RedGateway是一个是基于GoLang的，主要针对一些异构数据的采集任务而开发的采集通用数据采集平台。整体设计尽量考虑减少交互，便于现场人员配置使用。数据的输入和输出采用插件化开发，便于增删功能。



## 目录

<!-- toc -->

## 点表格式说明

```yaml
db40.dbx102.0:              # <required> 点名称，ASCII编码，便于跨语言使用, 可以自定义
  address: ""               # <required> point在设备中的物理地址，程序用来访问实时数据
  label: start              # <optional> 短名称
  name: 启动                 # <optional> 长名称
  unit: 单位                 # <optional> 单位字符串
  point_type:               # <optional> 点类型 0：模拟量，1：状态量, 2:整形量，3:字符串；默认为模拟量
  tags: ["test"]            # <optional> tag标记
  option:                   # <optional> 状态量映射关系
    1: 启动
    0: 停止
  parameter: 0.707423       # <optional> 模拟量系数
  extra:                    # <optional> 扩展字段
    key1: value1
    key2: value2
```



## 数据输出格式

每个数据源的每个采集周期输出数据格式如下：

fields:：数据字段
name：数据源标识
quality：数据质量
	1：Good
	2：Disconnect
	。。。：待扩展
	MaxUint8：Unknown
timestamp：ms时间戳

```json
[{
"fields":{
    "db40.dbd2":2.87,
    "db40.dbd6":3.46,
    "db40.dbw0":123,
    "db40.dbw112":1234,
    "db40.dbx102.0":1,
    "db40.dbx102.1":1,
    "db40.dbx102.7":0,
    "db40.dbx104.1":1
},
"name":"s7",
"quality":1,
"timestamp":1540261475146
}]
```



## 操作步骤

以 Modbus 数据源为例，输出到 Redis。

1. #### 启动，登陆

程序默认配置下在Mac系统会监听8080端口，在 Linux系统监听80端口；默认登陆账户为：admin/admin

![image-20190405193913554](https://ws1.sinaimg.cn/large/006tNc79ly1g1rzq89jvaj30jg0nk74n.jpg)

2. #### 配置输入

   ![image-20190405194040152](https://ws2.sinaimg.cn/large/006tNc79ly1g1rzroub0aj31z60jcq4v.jpg)

   点击 `添加数据源`，并选择指定采集类型

   ![image-20190405194142636](https://ws4.sinaimg.cn/large/006tNc79ly1g1rzsrpovnj30se0nm757.jpg)

   ![image-20190405194224510](https://ws2.sinaimg.cn/large/006tNc79ly1g1rzthbzkaj30ro0yawgc.jpg)

3. #### 配置点表

   ![image-20190405194308592](https://ws3.sinaimg.cn/large/006tNc79ly1g1rzu81yfcj31nu0eumyo.jpg)

   添加测试点表，示例：

   ![image-20190405194538083](https://ws1.sinaimg.cn/large/006tNc79ly1g1rzwtrzmuj31be0dyt9i.jpg)

   注1：由于 `address` 是必须字段，只添加 address 即可正常工作，实际使用时根据情况添加

   注2:  modbus地址索引从0开始

4. #### 配置输出

   ![image-20190405202423613](https://ws2.sinaimg.cn/large/006tNc79ly1g1s1177xamj31ne0dqdh0.jpg)

5. #### 检查输出数据

   ![image-20190405202736325](https://ws2.sinaimg.cn/large/006tNc79ly1g1s14jrx5sj31020h8aco.jpg)
