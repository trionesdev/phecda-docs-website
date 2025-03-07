---
sidebar_position: 1
---

# 边缘驱动介绍

## 概述

对于一些非标设备，特别是只能通过动态链接库方式进行通信的设备，或者是需要进行边缘计算的场景，我们提供了设备驱动SDK，让用户可以在边缘对接设备并进行相关处理后再上报至平台。

## 设计思路

边缘侧设备驱动，主要参考了`edgexfoundary`的实现。

通过物模型配置，进行设备实例的初始化。在功能层面实现了定时上报，主动获取，指令下发等功能。

## 配置文件说明

配置文件主要有全局配置文件，产品物模型配置文件和设备配置文件

### 全局配置文件

#### 属性说明

| 属性                     | 说明                    | 数据类型   |
|:-----------------------|:----------------------|:-------|
| MaxEventSize           | 上报事件体最大数，单位byte，0为不限制 | int    |
| Device                 |                       | object |
| Device.ProfilesDir     | profiles文件夹位置         | string |
| Device.DevicesDir      | devices文件夹位置          | string |
| MQTT                   |                       | object |
| MQTT.ClientId          | mqtt clientId         | string |
| MQTT.Protocol          | mqtt协议                | string |
| MQTT.Host              | mqtt host             | string |
| MQTT.Port              | mqtt 端口号              | int    |
| MQTT.QOS               |                       | int    |
| MQTT.Username          |                       | string |
| MQTT.Password          |                       | string |
| MQTT.ConnectionTimeout |                       | int    |
| MQTT.TopicPrefix       | topic前缀，所有的topic 都会加上 | string |

#### 范例

```yaml
MaxEventSize: 0 # value 0 unlimit the maximum event size that can be sent to message bus or core-data
Writable:
  LogLevel: INFO

Device:
  # These have common values (currently), but must be here for service local env overrides to apply when customized
  ProfilesDir: "./res/profiles"
  DevicesDir: "./res/devices"

MQTT:
  ClientId: "device-virtual"
  Protocol: "tcp"
  Host: "xxx.xxx.xxx.xxx"
  Port: 1883
  QOS: 0
  Username: "xxx"
  Password: "xxx"
  ConnectionTimeout: 30
  TopicPrefix: "phecda-test"

```

### 产品物模型配置文件

> 位置根据全局配置文件确定

物模型配置可直接在平台端生成

#### 属性说明

| 属性                                                   | 说明                          | 数据类型      |
|:-----------------------------------------------------|:----------------------------|:----------|
| productKey                                           | 产品Key                       | String    |
| name                                                 | 名称                          | String    |
| manufacturer                                         |                             | String    |
| model                                                | 模型名称                        | String    |
| labels                                               | 标签                          | String\[] |
| description                                          | 说明                          | String    |
| deviceProperties                                     | 设备属性                        | Object\[] |
| deviceProperties\[].identifier                       | 设备属性标识                      | String    |
| deviceProperties\[].name                             | 设备属性名称                      | String    |
| deviceProperties\[].description                      | 设备属性描述                      | String    |
| deviceProperties\[].properties                       | 设备属性的属性                     | Object    |
| deviceProperties\[].properties.valueType             | 设备属性的值类型                    | String    |
| deviceProperties\[].properties.readWrite             | 设备属性的读写\:R(只读)，W(只写)，RW(读写) | String    |
| deviceCommands                                       | 设备指令                        | Object\[] |
| deviceCommands\[].identifier                         | 设备指令标识                      | String    |
| deviceCommands\[].name                               | 设备指令名称                      | String    |
| deviceCommands\[].callType                           | 设备指令请求类型：Async(异步)，Sync(同步) | String    |
| deviceCommands\[].inputData                          | 设备指令入参                      | Object\[] |
| deviceCommands\[].inputData\[].identifier            | 设备指令入参标识                    | String    |
| deviceCommands\[].inputData\[].name                  | 设备指令入参名称                    | String    |
| deviceCommands\[].inputData\[].properties            | 设备指令入参属性                    | String    |
| deviceCommands\[].inputData\[].properties.valueType  | 设备指令入参属性值类型                 | String    |
| deviceCommands\[].outputData                         | 设备指令出参                      | Object\[] |
| deviceCommands\[].outputData\[].identifier           | 设备指令出参标识                    | String    |
| deviceCommands\[].outputData\[].name                 | 设备指令出参名称                    | String    |
| deviceCommands\[].outputData\[].properties           | 设备指令出参属性                    | String    |
| deviceCommands\[].outputData\[].properties.valueType | 设备指令出参属性值类型                 | String    |
| deviceEvents                                         | 设备事件                        | Object\[] |
| deviceEvents\[].identifier                           | 设备事件标识                      | String    |
| deviceEvents\[].name                                 | 设备事件名称                      | String    |
| deviceEvents\[].type                                 | 设备事件类型                      | String    |
| deviceEvents\[].outputData                           | 设备事件返回值                     | String    |
| deviceEvents\[].outputData\[].identifier             | 设备事件返回值标识                   | String    |
| deviceEvents\[].outputData\[].name                   | 设备事件返回值名称                   | String    |
| deviceEvents\[].outputData\[].properties             | 设备事件返回值属性                   | Object\[] |
| deviceEvents\[].outputData\[].properties.valueType   | 设备事件返回值属性值类型                | String    |

#### 范例

```yaml
productKey: 'trionesdev-test001'
name: "测试设备-Device"
manufacturer: "IOTech"
model: "Device-Virtual-01"
labels:
  - "device-virtual-example"
description: "Example of Device-Virtual"

deviceProperties:
  - identifier: "temperature"
    name: "温度"
    description: "温度"
    properties:
      valueType: "Int"
      readWrite: "R"


deviceCommands:
  - identifier: "get_temperature"
    name: "获取温度"
    callType: "Async"
    inputData:
      - identifier: "temperature"
        name: "温度"
        properties:
          valueType: "Int"
    outputData:
      - identifier: "temperature"
        name: "温度"
        properties:
          valueType: "Int"


deviceEvents:
  - identifier: "temperature_alarm"
    name: "温度报警"
    type: 'Warn'
    outputData:
      - identifier: "temperature"
        name: "温度"
        properties:
          valueType: "Int"

```

### 设备配置文件

> 位置根据全局配置文件确定

#### 属性说明

| 属性                                     | 说明                                   | 数据类型                             |
|:---------------------------------------|:-------------------------------------|:---------------------------------|
| deviceList                             | 设备列表                                 | object\[]                        |
| deviceList\[].name                     | 设备名称                                 |                                  |
| deviceList\[].profileName              | 设备物模型名称，需要存在于之前定义的物模型中               |                                  |
| deviceList\[].description              | 设备描述                                 |                                  |
| deviceList\[].labels                   | 设备标签                                 | string\[]                        |
| deviceList\[].tags                     | 设备标签，该标签会自动添加到事件标签中                  |                                  |
| deviceList\[].protocols                | 协议                                   | map\<string,map\<string,object>> |
| deviceList\[].autoEvents               | 自动事件                                 | object\[]                        |
| deviceList\[].autoEvents\[].interval   | 自动事件-间隔                              | string                           |
| deviceList\[].autoEvents\[].onChange   | 自动事件-是否变化才上报                         | bool                             |
| deviceList\[].autoEvents\[].sourceName | 自动事件-源，deviceResource或者deviceCommand | string                           |

#### 范例

```yaml
deviceList:
  - name: device_0001
    productKey: trionesdev-test001
    description: Example of Device Virtual
    labels:
      - device-virtual-example
    tags:
      env: test
    protocols: { }
    autoEvents:
      - interval: 10s
        onChange: false
        identifier: temperature
```

### 数据类型

| 类型          | 说明     |
|:------------|:-------|
| Bool        | 布尔值    |
| String      | 字符串    |
| Int         | 整数型    |
| Long        | 长整数    |
| Float       | 浮点数    |
| Double      | 双精度浮点数 |
| Struct      | 结构体    |
| BoolArray   | 布尔数组   |
| StringArray | 字符串数组  |
| IntArray    | 整数数组   |
| LongArray   | 长整数数组  |
| FloatArray  | 浮点数数组  |
| DoubleArray | 双精度数组  |
| StructArray | 结构体数组  |

