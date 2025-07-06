# Webhook 连接与消息处理指南

## 目录

1. [简介](#简介)
2. [Webhook 配置](#webhook-配置)
3. [消息接收](#消息接收)
4. [消息格式](#消息格式)
5. [消息处理](#消息处理)
6. [安全性](#安全性)
7. [错误处理](#错误处理)
8. [最佳实践](#最佳实践)
9. [示例代码](#示例代码)
10. [常见问题](#常见问题)

## 简介

Webhook 是一种基于 HTTP 的回调机制，允许我们的系统在特定事件发生时，向您预先配置的 URL 发送实时通知。通过 Webhook，您可以接收微信消息、状态变更等事件，实现系统间的实时数据同步。

### 主要功能

- 实时接收微信消息（文本、图片、语音、视频等）
- 接收群组消息和私聊消息
- 接收状态变更通知（登录状态、好友请求等）
- 支持消息过滤和自定义配置

## Webhook 配置

### 配置步骤

1. **创建 Webhook 配置**

   通过 API 创建 Webhook 配置：

   ```http
   POST /v1/webhook/Config
   ```

   请求体：
   ```json
   {
     "URL": "https://your-server.com/webhook/receiver",
     "Secret": "your_secret_key",
     "Enabled": true,
     "Timeout": 10,
     "RetryCount": 3,
     "MessageTypes": ["1", "3", "34", "43", "47", "49"],
     "IncludeSelfMessage": false
   }
   ```

   参数说明：
   - `URL`: 接收消息的服务器地址
   - `Secret`: 用于消息签名验证的密钥
   - `Enabled`: 是否启用该 Webhook
   - `Timeout`: 请求超时时间（秒）
   - `RetryCount`: 失败重试次数
   - `MessageTypes`: 接收的消息类型（1=文本, 3=图片, 34=语音, 43=视频, 47=表情, 49=链接）
   - `IncludeSelfMessage`: 是否接收自己发送的消息

2. **查看 Webhook 配置**

   ```http
   GET /v1/webhook/List
   ```

3. **测试 Webhook 连接**

   ```http
   GET /v1/webhook/Test
   ```

4. **更新 Webhook 配置**

   ```http
   PUT /v1/webhook/Update
   ```

## 消息接收

### 服务器要求

1. **公网可访问**：您的服务器必须能够从公网访问
2. **支持 HTTPS**：建议使用 HTTPS 确保安全性
3. **响应及时**：服务器应在 5 秒内响应请求，否则会被视为失败

### 接收流程

1. 当事件发生时，系统会向您配置的 URL 发送 POST 请求
2. 您的服务器需要返回 HTTP 状态码 200 表示成功接收
3. 如果返回其他状态码或超时，系统会根据配置的重试次数进行重试

## 消息格式

### 请求头

```
Content-Type: application/json
X-Webhook-Timestamp: 1625123456
X-Webhook-Signature: abcdef1234567890...
```

### 请求体

```json
{
  "key": "wxid_abcdefg123456",
  "msgId": "7654321098765432123",
  "timestamp": 1625123456,
  "fromUser": "wxid_sender12345",
  "toUser": "wxid_receiver12345",
  "msgType": 1,
  "content": "你好，这是一条测试消息",
  "isSelfMsg": false,
  "createTime": 1625123450,
  "isHistory": false
}
```

### 字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| key | string | 微信账号唯一标识 |
| msgId | string | 消息唯一ID |
| timestamp | int64 | 消息推送时间戳 |
| fromUser | string | 发送者ID |
| toUser | string | 接收者ID |
| msgType | int | 消息类型 |
| content | object | 消息内容，根据消息类型不同而不同 |
| isSelfMsg | boolean | 是否为自己发送的消息 |
| createTime | int64 | 消息创建时间戳 |
| isHistory | boolean | 是否为历史消息 |

### 消息类型说明

| msgType | 说明 | content 格式 |
|---------|------|--------------|
| 1 | 文本消息 | 字符串 |
| 3 | 图片消息 | 图片URL或Base64 |
| 34 | 语音消息 | 语音URL或Base64 |
| 43 | 视频消息 | 视频URL或Base64 |
| 47 | 表情消息 | 表情URL或Base64 |
| 49 | 链接消息 | 包含标题、描述、URL等的对象 |
| 10000 | 系统通知 | 字符串 |
| 10002 | 撤回消息 | 字符串 |

## 消息处理

### 验证签名

每个 Webhook 请求都包含签名，您应该验证签名以确保请求的合法性：

```python
import hmac
import hashlib

def verify_signature(request_body, timestamp, signature, secret):
    # 组合待签名字符串
    string_to_sign = timestamp + ":" + request_body
    
    # 计算签名
    computed_signature = hmac.new(
        secret.encode('utf-8'),
        string_to_sign.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()
    
    # 比较签名
    return hmac.compare_digest(computed_signature, signature)
```

### 处理不同类型的消息

根据 `msgType` 字段处理不同类型的消息：

```python
def process_message(message):
    msg_type = message.get("msgType")
    
    if msg_type == 1:  # 文本消息
        process_text_message(message)
    elif msg_type == 3:  # 图片消息
        process_image_message(message)
    elif msg_type == 34:  # 语音消息
        process_voice_message(message)
    elif msg_type == 43:  # 视频消息
        process_video_message(message)
    elif msg_type == 47:  # 表情消息
        process_emoji_message(message)
    elif msg_type == 49:  # 链接消息
        process_link_message(message)
    elif msg_type == 10000:  # 系统通知
        process_system_notification(message)
    elif msg_type == 10002:  # 撤回消息
        process_recall_message(message)
    else:
        process_unknown_message(message)
```

### 消息去重

系统已实现消息去重机制，但您仍应在自己的服务中实现消息去重，以防止重复处理：

```python
processed_messages = set()

def is_duplicate(message_id):
    if message_id in processed_messages:
        return True
    
    processed_messages.add(message_id)
    # 维护一个固定大小的集合，避免内存泄漏
    if len(processed_messages) > 10000:
        processed_messages.pop()
    
    return False

def process_message(message):
    message_id = message.get("msgId")
    
    if is_duplicate(message_id):
        print(f"跳过重复消息: {message_id}")
        return
    
    # 处理消息...
```

## 安全性

### 签名验证

始终验证请求签名，确保请求来自合法来源。

### HTTPS

强烈建议使用 HTTPS 协议保护数据传输安全。

### IP 白名单

可以设置 IP 白名单，只接受来自特定 IP 地址的请求。

### 敏感数据处理

妥善处理和存储接收到的敏感数据，遵守相关的数据保护法规。

## 错误处理

### 重试机制

系统会根据配置的重试次数，在请求失败时进行重试。重试间隔会随着重试次数增加而增加（指数退避）。

### 日志记录

记录所有 Webhook 请求和响应，以便排查问题：

```python
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger("webhook")

def handle_webhook(request):
    try:
        # 记录请求
        logger.info(f"Received webhook: {request.body}")
        
        # 处理请求
        process_message(request.json())
        
        # 记录成功
        logger.info("Webhook processed successfully")
        
        return {"status": "success"}, 200
    except Exception as e:
        # 记录错误
        logger.error(f"Error processing webhook: {str(e)}", exc_info=True)
        
        return {"status": "error", "message": str(e)}, 500
```

## 最佳实践

1. **快速响应**：尽快返回 HTTP 200 状态码，将耗时操作放入后台任务队列
2. **消息去重**：实现消息去重机制，避免重复处理
3. **异步处理**：使用异步处理框架提高并发能力
4. **监控告警**：设置监控和告警，及时发现问题
5. **定期清理**：定期清理历史数据，避免存储空间耗尽
6. **容错处理**：妥善处理各种异常情况，确保服务稳定性

## 示例代码

### Python (Flask) 示例

```python
from flask import Flask, request, jsonify
import hmac
import hashlib
import json
import time
import threading
import queue

app = Flask(__name__)

# 消息队列
message_queue = queue.Queue()

# 配置
WEBHOOK_SECRET = "your_secret_key"
PROCESSED_MESSAGES = set()
MAX_QUEUE_SIZE = 1000

# 消息处理线程
def message_processor():
    while True:
        try:
            message = message_queue.get(timeout=1)
            # 处理消息
            process_message(message)
            message_queue.task_done()
        except queue.Empty:
            time.sleep(0.1)
        except Exception as e:
            print(f"Error processing message: {e}")

# 启动处理线程
threading.Thread(target=message_processor, daemon=True).start()

@app.route('/webhook/receiver', methods=['POST'])
def webhook_receiver():
    # 获取请求数据
    timestamp = request.headers.get('X-Webhook-Timestamp')
    signature = request.headers.get('X-Webhook-Signature')
    body = request.get_data(as_text=True)
    
    # 验证签名
    if not verify_signature(body, timestamp, signature, WEBHOOK_SECRET):
        return jsonify({"status": "error", "message": "Invalid signature"}), 401
    
    # 解析消息
    try:
        message = json.loads(body)
    except json.JSONDecodeError:
        return jsonify({"status": "error", "message": "Invalid JSON"}), 400
    
    # 检查消息ID
    message_id = message.get("msgId")
    if not message_id:
        return jsonify({"status": "error", "message": "Missing message ID"}), 400
    
    # 检查是否重复消息
    if message_id in PROCESSED_MESSAGES:
        return jsonify({"status": "success", "message": "Duplicate message"}), 200
    
    # 添加到已处理集合
    PROCESSED_MESSAGES.add(message_id)
    if len(PROCESSED_MESSAGES) > MAX_QUEUE_SIZE:
        PROCESSED_MESSAGES.pop()
    
    # 将消息加入队列异步处理
    try:
        message_queue.put(message, block=False)
    except queue.Full:
        return jsonify({"status": "error", "message": "Server busy"}), 503
    
    # 立即返回成功
    return jsonify({"status": "success"}), 200

def process_message(message):
    # 根据消息类型处理
    msg_type = message.get("msgType")
    
    if msg_type == 1:  # 文本消息
        handle_text_message(message)
    elif msg_type == 3:  # 图片消息
        handle_image_message(message)
    # 处理其他消息类型...

def handle_text_message(message):
    content = message.get("content")
    from_user = message.get("fromUser")
    print(f"收到来自 {from_user} 的文本消息: {content}")
    # 处理文本消息的业务逻辑...

def handle_image_message(message):
    content = message.get("content")
    from_user = message.get("fromUser")
    print(f"收到来自 {from_user} 的图片消息: {content}")
    # 处理图片消息的业务逻辑...

def verify_signature(body, timestamp, signature, secret):
    if not all([body, timestamp, signature, secret]):
        return False
    
    string_to_sign = f"{timestamp}:{body}"
    computed_signature = hmac.new(
        secret.encode('utf-8'),
        string_to_sign.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()
    
    return hmac.compare_digest(computed_signature, signature)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### Node.js (Express) 示例

```javascript
const express = require('express');
const crypto = require('crypto');
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser.json());

// 配置
const WEBHOOK_SECRET = 'your_secret_key';
const PROCESSED_MESSAGES = new Set();
const MAX_SET_SIZE = 10000;

// 验证签名
function verifySignature(body, timestamp, signature, secret) {
  if (!body || !timestamp || !signature || !secret) {
    return false;
  }
  
  const stringToSign = `${timestamp}:${JSON.stringify(body)}`;
  const computedSignature = crypto
    .createHmac('sha256', secret)
    .update(stringToSign)
    .digest('hex');
  
  return crypto.timingSafeEqual(
    Buffer.from(computedSignature),
    Buffer.from(signature)
  );
}

// 消息处理
function processMessage(message) {
  const msgType = message.msgType;
  
  switch (msgType) {
    case 1:
      handleTextMessage(message);
      break;
    case 3:
      handleImageMessage(message);
      break;
    // 处理其他消息类型...
    default:
      console.log(`未知消息类型: ${msgType}`);
  }
}

function handleTextMessage(message) {
  const content = message.content;
  const fromUser = message.fromUser;
  console.log(`收到来自 ${fromUser} 的文本消息: ${content}`);
  // 处理文本消息的业务逻辑...
}

function handleImageMessage(message) {
  const content = message.content;
  const fromUser = message.fromUser;
  console.log(`收到来自 ${fromUser} 的图片消息: ${content}`);
  // 处理图片消息的业务逻辑...
}

// Webhook 接收端点
app.post('/webhook/receiver', (req, res) => {
  // 获取请求数据
  const timestamp = req.headers['x-webhook-timestamp'];
  const signature = req.headers['x-webhook-signature'];
  const body = req.body;
  
  // 验证签名
  if (!verifySignature(body, timestamp, signature, WEBHOOK_SECRET)) {
    return res.status(401).json({ status: 'error', message: 'Invalid signature' });
  }
  
  // 检查消息ID
  const messageId = body.msgId;
  if (!messageId) {
    return res.status(400).json({ status: 'error', message: 'Missing message ID' });
  }
  
  // 检查是否重复消息
  if (PROCESSED_MESSAGES.has(messageId)) {
    return res.status(200).json({ status: 'success', message: 'Duplicate message' });
  }
  
  // 添加到已处理集合
  PROCESSED_MESSAGES.add(messageId);
  if (PROCESSED_MESSAGES.size > MAX_SET_SIZE) {
    const firstItem = PROCESSED_MESSAGES.values().next().value;
    PROCESSED_MESSAGES.delete(firstItem);
  }
  
  // 异步处理消息
  setImmediate(() => {
    try {
      processMessage(body);
    } catch (error) {
      console.error(`处理消息错误: ${error}`);
    }
  });
  
  // 立即返回成功
  return res.status(200).json({ status: 'success' });
});

// 启动服务器
const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
  console.log(`Webhook 服务器运行在端口 ${PORT}`);
});
```

## 常见问题

### 1. 为什么我没有收到 Webhook 消息？

可能的原因：
- Webhook URL 不可访问（检查防火墙、网络设置）
- 服务器响应超时（确保在 5 秒内返回响应）
- 签名验证失败（检查密钥是否正确）
- 消息类型过滤（检查 MessageTypes 配置）

### 2. 如何处理大量并发消息？

- 使用异步处理框架（如 Celery、Bull 等）
- 实现消息队列（RabbitMQ、Redis 等）
- 水平扩展接收服务器
- 优化数据库操作

### 3. 如何确保消息不丢失？

- 实现消息持久化存储
- 使用可靠的消息队列
- 实现确认机制
- 定期检查消息处理状态

### 4. 如何测试 Webhook 接收功能？

- 使用 `/v1/webhook/Test` API 发送测试消息
- 使用工具如 Postman、curl 模拟请求
- 设置本地测试环境（使用 ngrok 等工具暴露本地服务）

### 5. 为什么会收到重复消息？

- 系统重试机制（请求失败时会重试）
- 网络问题导致重复发送
- 系统重启后重新处理未确认消息

### 6. 如何优化 Webhook 性能？

- 快速响应，异步处理
- 使用连接池
- 实现缓存机制
- 定期清理过期数据
- 使用高效的数据结构和算法 