---
title: NodeRed OPC相关
date: 2019-04-05 18:16:05
categories: 数据接入
tags: 
- node-red
- opc
---

​	node-red是用nodejs开发的，流式编程的数据采集与处理平台。使用鼠标拖动的方式配置数据流向图
​	常用的物联网相关的数据源主要有OPC DA，Modbus TCP，WebSocket，以及其他非标准的网络协议
​	常用的数据输出主要有RabbitMQ、Redis，PostgreSQL等，将采集到的数据处理成标准格式后输出到预定的输出接口

## 目录
<!-- toc -->


## 输出格式说明

```json
{
  "name": "16_opc_1_rslinx",
  "timestamp": 1554192662,
  "fields": [
    {
      "address": "[TCNY]COMM_FAULT_M229",
    	"value": 2,
    	"quality": "good"
  	}, 
    {
    	"address": "[TCNY]Local:9:I.Ch3Data",
    	"value": 1.56,
    	"quality": "good"
  	}
  ]
}
```

name: 数据源名称
timestamp：数据采集时间戳(ms)
fields：每个采集周期内可采集多个测点，数量视实际情况而定，理论上无上限



## OPC采集组件说明

OPC 采集节点包含1个输入端口和3个输出端口
一. 输入内容为前一个node输出的待采集的opc tag，示例：["tag1", "tag2", "tag3"]
二. 输入和输出的array是对应的，如果OPC Server中不存在该点， 则返回空值
三. 输出内容有3部分：
  **标准输出（payload）**
  - 如果类型选择的是read,则payload中的元素包含以下字段:
    - `ID` - Tag名称 - string
    - `Value` - Tag对应的值 number/boolean/string/array
    - `Quality` - 消息的质量 - string
    - `Timestamp` - 时间戳 - string
  - 如果类型选择的是property,则payload中的元素包含以下字段:
    - `ID` - Tag名称 - string
    - `Canonical DataType` - 数据类型 - string
    - `Value` - Tag对应的值 number/boolean/string/array
    - `Quality` - 消息的质量 - string
    - `Timestamp` - 时间戳 - string
    - `Access Rights` - 访问权限 - string
    - `Server Scan Rate` - 服务器扫描速率 - string
    - `EU Type` - 工程单位的类型 - string
    - `EUInfo` - 工程单位的信息 - string
    - `Description` - 描述 - string

**错误输出**
  - payload：执行错误的信息
  - rc：只有在property和write的时候存在

**返回码**
  - payload：包含返回码, 可能会存在 `message`, `signal` 属性


## OPC 采集操作步骤
1. #### 配置OPC Server 模拟器

![image-20190405163840550](https://ws3.sinaimg.cn/large/006tNc79ly1g1ruidvr9fj30e20egjsu.jpg)

![image-20190405163915252](https://ws1.sinaimg.cn/large/006tNc79ly1g1ruiwamm7j311w0ba0ul.jpg)

2. #### 在Channel下新建一个测试用Device

![image-20190405164056983](https://ws4.sinaimg.cn/large/006tNc79ly1g1ruknjsl2j31000kyq60.jpg)

3. #### 任意建立tag测试

![image-20190405164225616](https://ws2.sinaimg.cn/large/006tNc79ly1g1rum6mjjcj30sq0kq412.jpg)

4. #### 整理标准格式点表：

```json
{
	"a":{
		"point_type":1,
		"unit":"",
		"address":"Channel1.NodeRedDevice.a", #address 为必须字段
		"name":"a",
		"label":"a"
	}
}
```

5. #### 添加inject节点并添加点表内容：

![image-20190405164646971](https://ws4.sinaimg.cn/large/006tNc79ly1g1ruqoa304j30ry0nqdie.jpg)

6. #### 添加点表处理函数

   函数功能为提取必要字段 `address` ，并保存其他字段到上下文中

![image-20190405164835909](https://ws4.sinaimg.cn/large/006tNc79ly1g1rusmfmhtj31i00tcjv9.jpg)

7. #### 配置采集程序：

![image-20190405165150684](https://ws2.sinaimg.cn/large/006tNc79ly1g1ruw0ie2nj31d70u0n60.jpg)

8. #### 添加debug节点测试

![image-20190405165313803](https://ws4.sinaimg.cn/large/006tNc79ly1g1ruxgs2ztj31hi0u0437.jpg)
