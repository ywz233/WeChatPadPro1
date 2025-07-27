# RabbitMQ 配置说明

## 问题描述
发送一条微信消息时，系统会产生大量的RabbitMQ队列推送，这可能导致：
- 消息重复推送
- 系统资源浪费
- 日志混乱
- 性能下降

## 原因分析

### 1. 双重推送机制
系统存在两套推送机制：
- **直接推送**：在消息处理的最上游，直接推送到 `wx_messages_{uuid}` 队列
- **批量推送**：通过 `PublishSyncMsgWxMessage` 函数，推送到 `{uuid}_wx_sync_msg_topic` 或全局 `Topic`

### 2. 连接重试机制
每次推送失败都会重新连接，导致频繁的连接初始化。

### 3. 消息累积
短时间内处理多条消息，每条消息都触发多次推送。

## 解决方案

### 1. 避免重复推送
已优化代码，当RabbitMQ启用时，只使用直接推送方式，避免重复推送。

### 2. 优化连接管理
- 只在连接确实断开时才重连
- 减少不必要的重试次数
- 优化错误处理逻辑

### 3. 配置建议

#### 环境变量配置
```bash
# 启用RabbitMQ
RABBIT_MQ_ENABLED=true

# RabbitMQ连接URL
RABBIT_MQ_URL=amqp://username:password@host:port/

# 是否按微信ID发布消息（建议设为true）
NEWS_SYN_WXID=true

# 是否跳过Redis操作（RabbitMQ模式下建议设为true）
RABBIT_SKIP_REDIS=true
```

#### 队列命名规范
- 直接推送队列：`wx_messages_{uuid}`
- 批量推送队列：`{uuid}_wx_sync_msg_topic`
- 全局队列：`wx_sync_msg_topic`

### 4. 监控建议

#### 日志监控
关注以下日志模式：
- `[RabbitMQ] 开始推送消息 #X: topic=wx_messages_xxx`
- `[RabbitMQ] 推送消息成功 #X: topic=wx_messages_xxx`
- `[RabbitMQ] 连接初始化成功`

#### 性能指标
- 消息推送延迟
- 队列长度
- 连接状态
- 错误率

### 5. 故障排除

#### 常见问题
1. **消息重复推送**
   - 检查是否同时启用了直接推送和批量推送
   - 确认 `RABBIT_MQ_ENABLED` 配置正确

2. **连接频繁重试**
   - 检查网络连接稳定性
   - 确认RabbitMQ服务器状态
   - 查看连接池配置

3. **队列积压**
   - 检查消费者处理速度
   - 确认队列绑定正确
   - 监控队列长度

#### 调试命令
```bash
# 查看RabbitMQ队列状态
rabbitmqctl list_queues

# 查看连接状态
rabbitmqctl list_connections

# 查看交换机状态
rabbitmqctl list_exchanges
```

## 最佳实践

1. **单一推送策略**：选择一种推送方式，避免重复推送
2. **连接池管理**：合理配置连接池大小和超时时间
3. **错误处理**：实现优雅的错误处理和重试机制
4. **监控告警**：设置适当的监控和告警机制
5. **日志管理**：合理配置日志级别，避免日志过多

## 更新日志

### v861.202507021.00
- 优化消息推送逻辑，避免重复推送
- 改进连接管理，减少频繁重连
- 增强错误处理和日志记录
- 添加配置说明文档 