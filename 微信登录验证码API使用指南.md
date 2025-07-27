# 微信登录验证码 API 接口文档

## 概述

本文档详细说明微信登录验证码的完整使用流程，包括二维码获取、验证码提交、状态检测等接口。WeChatPadPro 提供了自动化的验证码处理功能，无需手动获取ticket和data62参数。

## 功能特性

- ✅ 支持多种登录方式（二维码、验证码、短信等）
- ✅ 自动处理验证码提交（无需手动获取ticket）
- ✅ 实时状态检测
- ✅ RabbitMQ消息队列支持
- ✅ 多账号管理
- ✅ 自动Token刷新

## 登录验证码使用流程

### 1. 获取微信登录二维码（绕过验证码）

**接口**: `POST /api/login/qr/newx`

**请求参数**:
```json
{
  "proxy": "http://proxy.example.com:8080",  // 可选，代理设置
  "deviceName": "iPhone",                    // 可选，设备名称
  "deviceId": "123456789"                   // 可选，设备ID
}
```

**响应示例**:
```json
{
  "code": 200,
  "success": true,
  "message": "获取二维码成功",
  "data": {
    "uuid": "abc123def456",
    "qrcode": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA...",
    "qrcodeUrl": "https://login.weixin.qq.com/qrcode/abc123def456",
    "expireTime": 300,
    "deviceId": "123456789",
    "data62": "base64encodeddata..."
  }
}
```

### 2. 自动处理验证码提交

**接口**: `POST /api/login/AutoVerificationcode`

**请求参数**:
```json
{
  "uuid": "abc123def456",    // 设备key（从步骤1获取）
  "code": "123456"           // 验证码（用户输入）
}
```

**说明**:
- `Data62` 和 `Ticket` 参数无需提供，后端会自动处理
- 系统会自动从Redis或状态缓存中获取ticket
- 如果data62为空，系统会自动生成

**响应示例**:
```json
{
  "code": 200,
  "success": true,
  "message": "验证码提交成功",
  "data": {
    "loginResult": "success",
    "userInfo": {
      "wxid": "wxid_abc123",
      "nickname": "用户昵称",
      "avatar": "头像URL"
    }
  }
}
```

### 3. 检测扫码状态

**接口**: `GET /api/login/CheckLoginStatus?key=abc123def456`

**响应示例**:
```json
{
  "code": 200,
  "success": true,
  "message": "扫码状态",
  "data": {
    "status": "scanned",
    "userInfo": {
      "wxid": "wxid_abc123",
      "nickname": "用户昵称"
    }
  }
}
```

## RabbitMQ 配置

### 1. 环境配置

在 `application.yml` 中配置RabbitMQ：

```yaml
rabbitmq:
  enabled: true
  url: "amqp://username:password@localhost:5672/"
  exchange: "wx_sys_exchangeName"
  heartbeat: 4s
```

### 2. 消息队列功能

RabbitMQ用于处理以下类型的消息：

- 登录状态变更通知
- 消息接收推送
- 设备状态更新
- 系统事件广播

### 3. 消息格式

**登录状态变更消息**:
```json
{
  "type": "login_status_change",
  "uuid": "abc123def456",
  "status": "online",
  "timestamp": 1640995200,
  "account": {
    "uuid": "abc123def456",
    "wxid": "wxid_abc123",
    "nickname": "用户昵称",
    "state": 1
  }
}
```

**消息接收推送**:
```json
{
  "type": "message_received",
  "uuid": "abc123def456",
  "message": {
    "fromUserName": "wxid_sender",
    "toUserName": "wxid_receiver",
    "content": "消息内容",
    "msgType": 1,
    "timestamp": 1640995200
  }
}
```

## 完整登录流程示例

### 步骤1: 获取二维码
```bash
curl -X POST "http://localhost:8080/api/login/qr/newx" \
  -H "Content-Type: application/json" \
  -d '{
    "deviceName": "iPhone",
    "deviceId": "123456789"
  }'
```

### 步骤2: 用户扫码并输入验证码
```bash
curl -X POST "http://localhost:8080/api/login/AutoVerificationcode" \
  -H "Content-Type: application/json" \
  -d '{
    "uuid": "abc123def456",
    "code": "123456"
  }'
```

### 步骤3: 检测登录状态
```bash
curl -X GET "http://localhost:8080/api/login/CheckLoginStatus?key=abc123def456"
```

## 错误处理

### 常见错误码

| 错误码 | 说明 | 解决方案 |
|--------|------|----------|
| -3 | 需要提交验证码 | 使用AutoVerificationcode接口提交验证码 |
| 300 | 状态不存在 | 重新获取二维码 |
| 400 | 参数错误 | 检查请求参数 |
| 500 | 系统错误 | 查看服务器日志 |

### 验证码相关错误

1. **验证码已过期**
   - 重新获取二维码
   - 确保在有效期内完成验证

2. **ticket获取失败**
   - 系统会自动重试
   - 检查Redis连接状态

3. **data62生成失败**
   - 系统会自动处理
   - 检查设备信息完整性

## 开发注意事项

### 1. 安全性
- 所有接口都需要有效的授权码
- 验证码有有效期限制
- 支持IP白名单配置

### 2. 性能优化
- 使用Redis缓存状态信息
- RabbitMQ异步处理消息
- 连接池管理

### 3. 监控告警
- 登录成功率监控
- 验证码提交成功率
- RabbitMQ消息队列状态

## 更新日志

### v1.0.0 (2024-01-01)
- ✅ 新增自动验证码处理功能
- ✅ 集成RabbitMQ消息队列
- ✅ 优化登录状态检测
- ✅ 支持多账号管理
- ✅ 添加自动Token刷新

### 待开发功能
- 🔄 支持更多登录方式
- 🔄 增强安全性验证
- 🔄 优化性能监控
- 🔄 添加管理后台

## 技术支持

如有问题，请联系技术支持团队或查看相关文档。

---

## 接口详细说明

### 1. 获取微信登录二维码（绕过验证码）

**接口地址**: `POST /api/login/qr/newx`

**接口描述**: 获取微信登录二维码，支持绕过验证码流程

**请求参数**:
```json
{
  "proxy": "http://proxy.example.com:8080",  // 可选，代理设置
  "deviceName": "iPhone",                    // 可选，设备名称
  "deviceId": "123456789"                   // 可选，设备ID
}
```

**请求头**:
```
Content-Type: application/json
Authorization: Bearer your_token_here
```

**响应示例**:
```json
{
  "code": 200,
  "success": true,
  "message": "获取二维码成功",
  "data": {
    "uuid": "abc123def456",
    "qrcode": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA...",
    "qrcodeUrl": "https://login.weixin.qq.com/qrcode/abc123def456",
    "expireTime": 300,
    "deviceId": "123456789",
    "data62": "base64encodeddata..."
  }
}
```

**错误响应**:
```json
{
  "code": 400,
  "success": false,
  "message": "请提供有效的授权码"
}
```

### 2. 自动处理验证码提交

**接口地址**: `POST /api/login/AutoVerificationcode`

**接口描述**: 自动处理验证码提交，无需手动获取ticket和data62

**请求参数**:
```json
{
  "uuid": "abc123def456",    // 设备key（从步骤1获取）
  "code": "123456"           // 验证码（用户输入）
}
```

**说明**:
- `Data62` 和 `Ticket` 参数无需提供，后端会自动处理
- 系统会自动从Redis或状态缓存中获取ticket
- 如果data62为空，系统会自动生成

**响应示例**:
```json
{
  "code": 200,
  "success": true,
  "message": "验证码提交成功",
  "data": {
    "loginResult": "success",
    "userInfo": {
      "wxid": "wxid_abc123",
      "nickname": "用户昵称",
      "avatar": "头像URL"
    }
  }
}
```

**错误响应**:
```json
{
  "code": 400,
  "success": false,
  "message": "验证码已过期或无效，请重新扫码登录"
}
```

### 3. 检测扫码状态

**接口地址**: `GET /api/login/CheckLoginStatus`

**接口描述**: 检测扫码登录状态

**请求参数**:
```
key=abc123def456  // 设备key（从步骤1获取）
```

**响应示例**:
```json
{
  "code": 200,
  "success": true,
  "message": "扫码状态",
  "data": {
    "status": "scanned",
    "userInfo": {
      "wxid": "wxid_abc123",
      "nickname": "用户昵称"
    }
  }
}
```

**需要验证码的响应**:
```json
{
  "code": -3,
  "success": false,
  "message": "请提交验证码后登录",
  "data": "ticket_value_here"
}
```

**无状态的响应**:
```json
{
  "code": 300,
  "data": null,
  "text": "不存在状态"
}
```

## 完整使用流程

### 步骤1: 获取二维码

```bash
curl -X POST "http://localhost:8080/api/login/qr/newx" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_token_here" \
  -d '{
    "deviceName": "iPhone",
    "deviceId": "123456789"
  }'
```

**响应**:
```json
{
  "code": 200,
  "success": true,
  "message": "获取二维码成功",
  "data": {
    "uuid": "abc123def456",
    "qrcode": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA...",
    "qrcodeUrl": "https://login.weixin.qq.com/qrcode/abc123def456",
    "expireTime": 300,
    "deviceId": "123456789",
    "data62": "base64encodeddata..."
  }
}
```

### 步骤2: 用户扫码并输入验证码

当用户扫码后，如果微信要求输入验证码，系统会返回需要验证码的状态。

**检测状态**:
```bash
curl -X GET "http://localhost:8080/api/login/CheckLoginStatus?key=abc123def456"
```

**响应（需要验证码）**:
```json
{
  "code": -3,
  "success": false,
  "message": "请提交验证码后登录",
  "data": "ticket_value_here"
}
```

**提交验证码**:
```bash
curl -X POST "http://localhost:8080/api/login/AutoVerificationcode" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer your_token_here" \
  -d '{
    "uuid": "abc123def456",
    "code": "123456"
  }'
```

**响应**:
```json
{
  "code": 200,
  "success": true,
  "message": "验证码提交成功",
  "data": {
    "loginResult": "success",
    "userInfo": {
      "wxid": "wxid_abc123",
      "nickname": "用户昵称",
      "avatar": "头像URL"
    }
  }
}
```

### 步骤3: 确认登录成功

```bash
curl -X GET "http://localhost:8080/api/login/CheckLoginStatus?key=abc123def456"
```

**响应**:
```json
{
  "code": 200,
  "success": true,
  "message": "扫码状态",
  "data": {
    "status": "scanned",
    "userInfo": {
      "wxid": "wxid_abc123",
      "nickname": "用户昵称"
    }
  }
}
```

## 错误码说明

| 错误码 | 说明 | 解决方案 |
|--------|------|----------|
| 200 | 成功 | - |
| -3 | 需要提交验证码 | 使用AutoVerificationcode接口提交验证码 |
| 300 | 状态不存在 | 重新获取二维码 |
| 400 | 参数错误 | 检查请求参数 |
| 401 | 授权失败 | 检查授权码 |
| 500 | 系统错误 | 查看服务器日志 |

## 状态码说明

### 登录状态

| 状态 | 说明 |
|------|------|
| `pending` | 等待扫码 |
| `scanned` | 已扫码 |
| `confirmed` | 已确认 |
| `expired` | 已过期 |
| `success` | 登录成功 |
| `failed` | 登录失败 |

### 验证码状态

| 状态 | 说明 |
|------|------|
| `need_verification` | 需要验证码 |
| `verification_success` | 验证码提交成功 |
| `verification_failed` | 验证码提交失败 |

## 安全说明

### 1. 授权验证

所有接口都需要有效的授权码，通过以下方式提供：

- **Query参数**: `?key=your_auth_key`
- **Header**: `Authorization: Bearer your_auth_key`

### 2. 参数验证

- `uuid` 必须与获取二维码时返回的uuid一致
- `code` 验证码必须是6位数字
- 所有参数都会进行格式验证

### 3. 时效性

- 二维码有效期为5分钟
- 验证码有效期为2分钟
- 登录状态缓存时间为10分钟

## 性能优化

### 1. 缓存机制

- 使用Redis缓存登录状态
- 使用内存缓存验证码信息
- 实现连接池管理

### 2. 异步处理

- 验证码提交异步处理
- 状态更新异步推送
- 消息队列处理

### 3. 监控告警

- 接口响应时间监控
- 错误率统计
- 成功率分析

## 开发示例

### JavaScript 示例

```javascript
// 获取二维码
async function getQRCode() {
  const response = await fetch('/api/login/qr/newx', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer your_token_here'
    },
    body: JSON.stringify({
      deviceName: 'iPhone',
      deviceId: '123456789'
    })
  });
  
  const result = await response.json();
  return result.data;
}

// 提交验证码
async function submitVerificationCode(uuid, code) {
  const response = await fetch('/api/login/AutoVerificationcode', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer your_token_here'
    },
    body: JSON.stringify({
      uuid: uuid,
      code: code
    })
  });
  
  const result = await response.json();
  return result;
}

// 检查登录状态
async function checkLoginStatus(uuid) {
  const response = await fetch(`/api/login/CheckLoginStatus?key=${uuid}`);
  const result = await response.json();
  return result;
}

// 完整流程示例
async function loginFlow() {
  try {
    // 1. 获取二维码
    const qrData = await getQRCode();
    console.log('二维码获取成功:', qrData);
    
    // 2. 轮询检查状态
    const checkStatus = async () => {
      const status = await checkLoginStatus(qrData.uuid);
      
      if (status.code === -3) {
        // 需要验证码
        const userCode = prompt('请输入验证码:');
        if (userCode) {
          const result = await submitVerificationCode(qrData.uuid, userCode);
          console.log('验证码提交结果:', result);
        }
      } else if (status.code === 200) {
        console.log('登录成功:', status.data);
        return;
      }
      
      // 继续轮询
      setTimeout(checkStatus, 2000);
    };
    
    checkStatus();
    
  } catch (error) {
    console.error('登录流程出错:', error);
  }
}
```

### Python 示例

```python
import requests
import time

class WeChatLogin:
    def __init__(self, base_url, auth_token):
        self.base_url = base_url
        self.headers = {
            'Content-Type': 'application/json',
            'Authorization': f'Bearer {auth_token}'
        }
    
    def get_qr_code(self):
        """获取二维码"""
        url = f"{self.base_url}/api/login/qr/newx"
        data = {
            "deviceName": "iPhone",
            "deviceId": "123456789"
        }
        
        response = requests.post(url, headers=self.headers, json=data)
        return response.json()
    
    def submit_verification_code(self, uuid, code):
        """提交验证码"""
        url = f"{self.base_url}/api/login/AutoVerificationcode"
        data = {
            "uuid": uuid,
            "code": code
        }
        
        response = requests.post(url, headers=self.headers, json=data)
        return response.json()
    
    def check_login_status(self, uuid):
        """检查登录状态"""
        url = f"{self.base_url}/api/login/CheckLoginStatus"
        params = {"key": uuid}
        
        response = requests.get(url, headers=self.headers, params=params)
        return response.json()
    
    def login_flow(self):
        """完整登录流程"""
        try:
            # 1. 获取二维码
            qr_result = self.get_qr_code()
            if qr_result['success']:
                uuid = qr_result['data']['uuid']
                print(f"二维码获取成功，UUID: {uuid}")
                
                # 2. 轮询检查状态
                while True:
                    status = self.check_login_status(uuid)
                    
                    if status['code'] == -3:
                        # 需要验证码
                        code = input("请输入验证码: ")
                        if code:
                            result = self.submit_verification_code(uuid, code)
                            print(f"验证码提交结果: {result}")
                    
                    elif status['code'] == 200:
                        print(f"登录成功: {status['data']}")
                        break
                    
                    time.sleep(2)
            else:
                print(f"获取二维码失败: {qr_result['message']}")
                
        except Exception as e:
            print(f"登录流程出错: {e}")

# 使用示例
if __name__ == "__main__":
    login = WeChatLogin("http://localhost:8080", "your_auth_token")
    login.login_flow()
```

## 更新日志

### v1.0.0 (2024-01-01)
- ✅ 新增自动验证码处理功能
- ✅ 优化二维码获取接口
- ✅ 完善状态检测机制
- ✅ 添加错误处理
- ✅ 支持多设备登录

### 待开发功能
- 🔄 支持更多验证码类型
- 🔄 增强安全性验证
- 🔄 优化性能监控
- 🔄 添加批量操作接口 