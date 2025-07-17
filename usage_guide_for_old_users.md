# WeChatPadPro 老用户使用指南

## 避免大量过期消息重复处理

對於長期使用 WeChatPadPro 的老用戶，我們強烈建議先運行 webhook-client.py 進行消息緩存處理，這可以有效避免大量過期消息重複處理的問題。

### 使用步驟

1. **設置 webhook-client.py**

   打開 `webhook-client.py` 文件，修改以下配置：
   
   ```python
   WEBHOOK_SECRET = "your_secret_key"  # 修改為與 webhook_config.json 中相同的密鑰
   INCLUDE_SELF_MESSAGE = True  # 是否包含自己發送的消息
   ```

2. **安裝依賴**

   webhook-client.py 需要安裝 Flask 庫：
   
   ```bash
   pip install flask
   ```

3. **先啓動 webhook-client**

   在啓動 WeChatPadPro 主程序之前，先運行 webhook-client.py：
   
   ```bash
   python webhook-client.py
   ```
   
   這將在本地啓動一個 webhook 服務器，監聽 8000 端口。

4. **配置 WeChatPadPro 使用本地 webhook**

   修改 `.env` 文件，添加或更新以下配置：
   
   ```
   WEBHOOK_URL=http://localhost:8000/webhook
   WEBHOOK_SECRET=your_secret_key
   ```

5. **啓動 WeChatPadPro**

   使用 `run.bat` 或 `run.sh` 啓動主程序。

### 優點

- **避免消息風暴**：初次運行或長時間未運行後重新啓動時，可能會收到大量歷史消息，使用 webhook-client 可以緩存這些消息。
- **日誌記錄**：webhook-client 會將所有接收到的消息記錄到 webhook.log 文件中，便於後續分析。
- **消息過濾**：可以通過修改 webhook-client.py 代碼來過濾不需要的消息類型。

### 疑難解答

- **端口衝突**：如果 8000 端口被佔用，可以修改 webhook-client.py 最後一行的端口號。
- **連接問題**：確保防火牆允許本地連接到 8000 端口。
- **日誌查看**：運行目錄下的 webhook.log 文件包含所有接收到的消息記錄。

## 進階用法

高級用戶可以擴展 webhook-client.py 的功能，例如：

- 添加消息處理邏輯
- 實現消息轉發到其他系統
- 基於消息內容觸發自動化操作

如需進一步幫助，請查閱完整文檔或聯繫技術支持。 