# 微信機器人Webhook接收服務

這是一個用於接收微信機器人訊息推送的Webhook服務，可以接收並處理來自WeChatPadPro的各類訊息通知。

## 功能特點

- 支持接收微信各種類型的訊息（文字、圖片、語音、視頻等）
- 支持簽名驗證，確保訊息安全性
- 詳細的日誌記錄
- 可自定義訊息處理邏輯
- 提供健康檢查接口

## 系統要求

- Python 3.6+
- 可從外部訪問的伺服器（如果需要公網訪問）

## 快速開始

### Windows用戶

直接運行`start_webhook.cmd`批處理文件即可快速啟動服務：

```
雙擊運行 start_webhook.cmd
```

### 手動啟動

1. 安裝依賴

```bash
pip install -r requirements.txt
```

2. 配置環境變量（可選）

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

3. 啟動服務

```bash
python webhook_server.py
```

## 配置說明

服務啟動後會顯示詳細的配置信息，包括如何在WeChatPadPro項目中配置對應的Webhook。

### 環境變量

| 變量名 | 說明 | 默認值 |
|-------|------|-------|
| PORT | 服務監聽端口 | 8000 |
| WEBHOOK_SECRET | 簽名密鑰 | your_secret_key |
| WEBHOOK_PATH | Webhook接收路徑 | /webhook |

### 在WeChatPadPro中配置

使用以下API配置Webhook：

```
POST /api/webhook/config?key=YOUR_WECHAT_KEY

{
  "url": "http://您的伺服器IP:8000/webhook",
  "secret": "your_secret_key",
  "enabled": true,
  "timeout": 10,
  "retryCount": 3,
  "messageTypes": [],  // 空數組表示接收所有類型的訊息
  "includeSelfMessage": true  // 設置為true可以接收自己發送的訊息
}
```

## 訊息處理

如需自定義訊息處理邏輯，請修改`webhook_server.py`文件中的`handle_message`函數：

```python
def handle_message(message):
    # 自定義處理邏輯
    # ...
    return True
```

## 日誌

服務運行日誌保存在`webhook.log`文件中，包含接收到的所有訊息詳情。

## 測試與狀態查詢

- 測試Webhook連通性：
  ```
  GET /api/webhook/test?key=YOUR_WECHAT_KEY
  ```

- 查看Webhook配置狀態：
  ```
  GET /api/webhook/status?key=YOUR_WECHAT_KEY
  ```

## 注意事項

- 確保伺服器可以從外部訪問（如需公網訪問，請配置端口轉發）
- 如果使用了簽名密鑰，確保與WeChatPadPro配置中的一致
- 建議在生產環境中使用HTTPS以確保通信安全 