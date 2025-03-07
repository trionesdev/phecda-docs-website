---
sidebar_position: 2
---
# MQTT处理
## 添加依赖
```xml
<dependency>
    <groupId>com.trionesdev.phecda</groupId>
    <artifactId>phecda-gateway-handler-mqtt</artifactId>
</dependency>
```
## 使用示例
在 core 模块下添加 `com.trionesdev.phecda.gateway.core.handle.mqtt` package。

创建 MqttDemoHandler 类
```javascript
@GatewayHandlerComponent(productKeys = {"mqtt1"})
public class MqttDemoHandler extends MqttGatewayProcessor {


    @Override
    public void sendCommand(@NotNull PhecdaCommand command) {
        this.publish("mqtt-test-command", new MqttMessage(JSON.toJSONBytes(command)));
    }

    @Override
    public void onConnectComplete(boolean reconnect) {
        this.subscribe("mqtt-test-properties", 0, (topic, message) -> {
            postProperties(JSON.parseObject(message.getPayload(), PhecdaEvent.class));
        });
        this.subscribe("mqtt-test-events", 0, (topic, message) -> {
            postProperties(JSON.parseObject(message.getPayload(), PhecdaEvent.class));
        });
    }
}

```
