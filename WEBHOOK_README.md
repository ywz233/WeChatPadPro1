# 微信机器人Webhook接收服务

这是一个用于接收微信机器人消息推送的Webhook服务，可以接收并处理来自WeChatPadPro的各类消息通知。

## 功能特点

- 支持接收微信各种类型的消息（文本、图片、语音、视频等）
- 支持签名验证，确保消息安全性
- 详细的日志记录
- 可自定义消息处理逻辑
- 提供健康检查接口

## 系统要求

- Python 3.6+
- 可从外部访问的服务器（如果需要公网访问）

## 快速开始

### Windows用户

直接运行`start_webhook.cmd`批处理文件即可快速启动服务：

```
双击运行 start_webhook.cmd
```

### 手动启动

1. 安装依赖

```bash
pip install -r requirements.txt
```

2. 配置环境变量（可选）

```bash
# Windows
set PORT=8000
set WEBHOOK_SECRET=your_secret_key
set WEBHOOK_PATH=/webhook

# Linux/Mac
export PORT=8000
export WEBHOOK_SECRET=your_secret_key
export WEBHOOK_PATH=/webhook
```

3. 启动服务

```bash
python webhook_server.py
```

## 配置说明

服务启动后会显示详细的配置信息，包括如何在WeChatPadPro项目中配置对应的Webhook。

### 环境变量

| 变量名 | 说明 | 默认值 |
|-------|------|-------|
| PORT | 服务监听端口 | 8000 |
| WEBHOOK_SECRET | 签名密钥 | your_secret_key |
| WEBHOOK_PATH | Webhook接收路径 | /webhook |

### 在WeChatPadPro中配置

使用以下API配置Webhook：

```
POST /api/webhook/config?key=YOUR_WECHAT_KEY

{
  "url": "http://您的服务器IP:8000/webhook",
  "secret": "your_secret_key",
  "enabled": true,
  "timeout": 10,
  "retryCount": 3,
  "messageTypes": [],  // 空数组表示接收所有类型的消息
  "includeSelfMessage": true  // 设置为true可以接收自己发送的消息
}
```

## 消息处理

如需自定义消息处理逻辑，请修改`webhook_server.py`文件中的`handle_message`函数：

```python
def handle_message(message):
    # 自定义处理逻辑
    # ...
    return True
```

## 日志

服务运行日志保存在`webhook.log`文件中，包含接收到的所有消息详情。

## 测试与状态查询

- 测试Webhook连通性：
  ```
  GET /api/webhook/test?key=YOUR_WECHAT_KEY
  ```

- 查看Webhook配置状态：
  ```
  GET /api/webhook/status?key=YOUR_WECHAT_KEY
  ```

## 注意事项

- 确保服务器可以从外部访问（如需公网访问，请配置端口转发）
- 如果使用了签名密钥，确保与WeChatPadPro配置中的一致
- 建议在生产环境中使用HTTPS以确保通信安全 