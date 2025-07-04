# WeChatPadPro

<div align="center">
  <img src="static/doc/4270252wc57e-4b66-9ae3-0a81fc001255.png" alt="WeChatPadPro" width="800px">
</div>

<h1 align="center">🚀 基於 WeChat Pad 協議的高級管理工具</h1>

<div align="center">
  <strong>🌐 <a href="https://wx.knowhub.cloud/docs/">在線演示系統</a> - 默認密碼: 12345</strong>
</div>

<p align="center">
  <a href="https://github.com/WeChatPadPro/WeChatPadPro">
    <img src="https://img.shields.io/badge/Version-v8059-blue?style=for-the-badge" alt="版本">
  </a>
  <a href="https://github.com/WeChatPadPro/WeChatPadPro">
    <img src="https://img.shields.io/badge/iOS-18.3.2-brightgreen?style=for-the-badge" alt="iOS">
  </a>
  <a href="https://t.me/+LK0JuqLxjmk0ZjRh">
    <img src="https://img.shields.io/badge/Telegram-交流群-blue?style=for-the-badge&logo=telegram" alt="Telegram">
  </a>
  <br>
  <a href="https://github.com/WeChatPadPro/WeChatPadPro/stargazers">
    <img src="https://img.shields.io/github/stars/WeChatPadPro/WeChatPadPro?style=for-the-badge" alt="Stars">
  </a>
  <a href="https://github.com/WeChatPadPro/WeChatPadPro/network/members">
    <img src="https://img.shields.io/github/forks/WeChatPadPro/WeChatPadPro?style=for-the-badge" alt="Forks">
  </a>
  <a href="https://github.com/WeChatPadPro/WeChatPadPro/issues">
    <img src="https://img.shields.io/github/issues/WeChatPadPro/WeChatPadPro?style=for-the-badge" alt="Issues">
  </a>
</p>

<div align="center">
  <strong>⭐️ 歡迎 Star，獲取專案最新動態！⭐️</strong>
</div>

<p align="center">
  <a href="#-專案介紹">📋 專案介紹</a> •
  <a href="#-功能特性">📝 功能特性</a> •
  <a href="#-使用指南與注意事項">📖 使用說明</a> •
  <a href="#-風控指南">🛡️ 風控指南</a> •
  <a href="#-贊助支持">💰 贊助支持</a>
</p>
## 🚀 v18.6 版本更新

> **[查看所有版本發布](https://github.com/WeChatPadPro/WeChatPadPro/releases)** - 獲取最新版本下載和更新說明

# WeChat API & iPad 協議使用說明與風控防範教程（2025 年更新）

本教程旨在協助開發者全面了解 WeChat API 平台與 iPad 協議的登入行為、介面使用限制、風控策略及預防措施，確保使用穩定、安全、高效。

## 📋 快速開始

在開始使用本專案之前，請務必：

1. 📚 仔細閱讀[風控指南](#-風控指南)，了解帳號安全事項
2. ⚙️ 按照[環境配置](#-環境配置)部署必要組件
3. 🔧 完成[軟體配置](#-軟體配置)並啟動服務
4. 🔒 遵循[登入注意事項](#登入注意事項)進行首次登入
5. 🧪 參考[測試指南](#-測試指南)進行功能測試

> ⚠️ **特別提醒**：新帳號請務必遵循[新帳號使用建議](#-重要提醒)，避免觸發風控！

## 📋 專案介紹

WeChatPadPro 是一個功能強大的 WeChat 管理工具，基於 WeChat Pad 協議開發。本專案致力於提供穩定、高效的 WeChat 自動化解決方案，支援多帳號管理、訊息處理、自動化任務等功能。

### 🌟 主要特點

- 🛡️ **安全可靠**: 採用最新的 WeChat Pad 協議，確保帳號安全
- 🔄 **自動化處理**: 支援訊息自動回覆、群管理等自動化操作
- 🎯 **精準控制**: 提供細粒度的功能控制和配置選項
- 🔌 **擴展性強**: 支援外掛系統，可自定義擴展功能
- 📊 **資料同步**: 支援多裝置資料同步，確保資訊統一

### 🎯 適用場景

- 👥 個人用戶：訊息管理、自動回覆、防撤回等
- 🏢 企業用戶：客戶管理、批量操作、資料分析等
- 🤖 開發者：二次開發、功能擴展、自動化整合等

### 📦 環境要求

- MySQL 5.7+ (推薦)
- Redis
- 穩定的網路環境
- 支援 Windows/Linux 系統

---

## 📝 功能特性

WeChatPadPro 是基於 WeChat Pad 協議的高級 WeChat 管理工具，支援以下功能：

<table>
<tr>
<td width="50%" valign="top">

### 🔹 基礎功能

- 💬 **訊息收發** - 文字、圖片、名片、動圖、檔案
- 👥 **好友管理** - 新增、刪除、清理殭屍粉
- 🔄 **朋友圈互動** - 發布、點讚、評論
- 💲 **WeChat 支付** - 轉帳、紅包
- 🔖 **小程式和名片分享**
- 📇 **通訊錄好友新增**
- ⭐ **WeChat 收藏**
- 🏷️ **標籤管理**

</td>
<td width="50%" valign="top">

### 🔹 增強功能

- 🔧 **MCP 增強功能**
  - 多協議適配：自動識別並適配不同版本 WeChat 協議
  - 自動化管理：通過配置實現請求自動通過、訊息同步
- 🤖 **自動化功能**
  - 自動搶紅包、訊息防撤回
  - 自動通過好友請求
  - 多群訊息同步
- 👑 **高級群管理**
  - 建群、拉人、踢人、邀請成員
  - 群公告發布、修改群名稱

</td>
</tr>
</table>

---

## 🚀 v18.6 版本更新

> **[查看所有版本發布](https://github.com/WeChatPadPro/WeChatPadPro/releases)** - 獲取最新版本下載和更新說明

### 1. 新增功能

- **授權碼管理優化**：改進了授權碼授權設備管理API接口
- **遠程Docker支持**：新增對遠程Docker環境的支持
- **多平台構建優化**：支持更多操作系統和CPU架構
- **資料庫連接增強**：優化了資料庫連接池和錯誤處理
- **Webhook配置增強**：支持更多Webhook觸發事件和自定義配置
- **檔案傳輸加速**：優化了大檔案傳輸性能和穩定性
- **驗證碼識別**：集成驗證碼自動識別功能，提高登入成功率

### 2. 性能優化

- 降低了CPU和記憶體佔用
- 優化了網路連接處理
- 提高了大規模訊息處理的效率
- 改進了錯誤處理和日誌記錄
- 加速了檔案傳輸和處理速度
- 提高了Webhook響應速度

### 3. 支持平台

WeChatPadPro v18.6 支持以下操作系統和CPU架構：

| 操作系統 | 支持的架構 | 檔案名 |
|---------|-----------|-------|
| Windows | AMD64 (x86_64) | wechatpadpro_v18.6_20250704_windows-amd64.zip |
| Windows | ARM64 | wechatpadpro_v18.6_20250704_windows-arm64.zip |
| macOS | AMD64 (Intel) | wechatpadpro_v18.6_20250704_macos-amd64.zip |
| macOS | ARM64 (M1/M2) | wechatpadpro_v18.6_20250704_macos-arm64.zip |
| Linux | AMD64 (x86_64) | wechatpadpro_v18.6_20250704_linux-amd64.zip |
| Linux | ARM64 | wechatpadpro_v18.6_20250704_linux-arm64.zip |
| Linux | MIPS64 | wechatpadpro_v18.6_20250704_linux-mips64.zip |
| Linux | MIPS64LE | wechatpadpro_v18.6_20250704_linux-mips64le.zip |
| Linux | PPC64 | wechatpadpro_v18.6_20250704_linux-ppc64.zip |
| Linux | PPC64LE | wechatpadpro_v18.6_20250704_linux-ppc64le.zip |
| Linux | RISC-V 64 | wechatpadpro_v18.6_20250704_linux-riscv64.zip |
| FreeBSD | AMD64 | wechatpadpro_v18.6_20250704_freebsd-amd64.zip |
| FreeBSD | ARM64 | wechatpadpro_v18.6_20250704_freebsd-arm64.zip |
| OpenBSD | AMD64 | wechatpadpro_v18.6_20250704_openbsd-amd64.zip |
| OpenBSD | ARM64 | wechatpadpro_v18.6_20250704_openbsd-arm64.zip |

### 4. 安裝說明

#### 系統要求

- 操作系統：Windows 7+、macOS 10.13+、Linux (內核 3.10+)、FreeBSD 12+、OpenBSD 6.8+
- 記憶體：至少 2GB RAM
- 儲存：至少 200MB 可用空間
- 網路：穩定的互聯網連接

#### 安裝步驟

1. **下載**：根據您的操作系統和CPU架構，從上表中選擇對應的壓縮包下載。
2. **解壓**：將下載的壓縮包解壓到您選擇的目錄。
3. **配置**：
   - 修改 `config.json` 檔案，設置必要的參數
   - 配置 `webhook_config.json` 檔案，設置Webhook相關參數
   - 設置環境變數（可選）：
     ```
     # Linux/macOS
     export ADMIN_KEY="您的管理密鑰"
     
     # Windows
     $env:ADMIN_KEY="您的管理密鑰"
     ```
4. **運行**：
   - Windows: 雙擊 `wechatpadpro.exe` 或在命令行中運行
   - Linux/macOS/BSD: 執行 `./wechatpadpro`

### 5. 功能使用說明

#### 檔案發送

WeChatPadPro 支持發送多種類型的檔案，包括圖片、視頻、文檔等。

```
POST /api/v1/message/sendFile
Content-Type: multipart/form-data

{
  "toUserName": "接收者微信ID",
  "filePath": "本地檔案路徑",  // 與fileData二選一
  "fileData": "base64編碼的檔案數據",  // 與filePath二選一
  "fileName": "檔案名稱",
  "fileType": "檔案類型"  // 可選值: image, video, file
}
```

#### Webhook配置

Webhook 可以將微信訊息即時推送到您指定的URL。配置方法：

1. 編輯 `webhook_config.json` 檔案：
```json
{
  "enabled": true,
  "url": "http://您的服務器地址/webhook/receiver",
  "events": ["message", "login", "logout", "friend_request"],
  "retry_count": 3,
  "retry_interval": 5,
  "secret_key": "您的密鑰"
}
```

2. 接收Webhook訊息的服務器需要處理POST請求，訊息格式為：
```json
{
  "event_type": "message",
  "timestamp": 1656789012,
  "data": {
    // 事件相關數據
  },
  "signature": "訊息簽名"
}
```

---

## 📖 WeChat 功能使用說明

WeChatPadPro 提供了豐富的 WeChat 功能控制命令，包括：

- **自動搶紅包功能**：控制搶紅包、設置延遲時間、過濾測試紅包等
- **訊息防撤回**：查看被對方撤回的訊息內容
- **好友管理**：自動通過驗證、新增好友後自動回覆
- **群管理命令**：踢人、拉黑、移出黑名單等操作
- **朋友圈互動**：自動點讚朋友圈功能

👉 [查看完整的 WeChat 功能使用說明](./WeChat功能使用說明.md)

---

## 📢 加入官方交流群

加入我們的官方交流群，獲取最新版本更新、技術支援和使用教程！

<table>
<tr>
<td width="50%" align="center">

### 💬 Telegram 交流群

<img src="./static/qrcode/diabao.JPG" width="180" alt="Telegram群二維碼">

🔗 **[點擊加入 Telegram](https://t.me/+LK0JuqLxjmk0ZjRh)**
- 成員數量: 203+ 位用戶
- 即時更新通知
- 技術問題解答
- 使用經驗分享

</td>
<td width="33%" align="center">

### 💼 微信付費專業群

<img src="https://raw.githubusercontent.com/WeChatPadPro/WeChatPadPro/refs/heads/main/static/qrcode/%E5%9B%BE%E7%89%87_20250619211139.jpg" width="180" alt="微信付費入群二維碼">

🔒 **掃碼付費加入專業交流群**
- VIP技術支持
- 專屬功能體驗
- 優先解決問題
- 高級使用技巧分享
</td>
<td width="50%" align="center">

### 🔔 釘釘交流群

<img src="./static/qrcode/IMG_3817.JPG" width="180" alt="釘釘群二維碼">

🔗 **[點擊加入釘釘](https://qr.dingtalk.com/action/joingroup?code=v1,k1,6uLDgyJodWIXoxwdwF3bYlv1GRj5uoC8waI6Z4PtIi0=&_dt_no_comment=1&origin=11)**
- 技術討論
- 問題解答
- 使用經驗分享

</td>
</tr>
</table>

---

## 💰 贊助支持

<div align="center">
  <br>
  <img src="https://upload.wikimedia.org/wikipedia/commons/b/ba/Stripe_Logo%2C_revised_2016.svg" width="180px" alt="Stripe Logo"/>
  <h3>您的支持是我們持續更新的動力</h3>
  
  <a href="https://book.stripe.com/00w6oHbPkc7D1rn0iJ0Jq06">
    <img src="https://img.shields.io/badge/-%F0%9F%92%B0%20%E9%BB%9E%E6%93%8A%E8%B4%8A%E5%8A%A9-00BB00?style=for-the-badge" height="60" width="400" alt="點擊贊助" />
  </a>
  <br><br>
</div>

<table>
<tr>
<td width="70%">

### 🌟 成為贊助者可獲得
- ✅ **優先技術支援** - 獲得開發者直接回應
- ✅ **新功能優先體驗** - 提前體驗最新功能
- ✅ **客製化需求支援** - 您的需求將被優先考慮
- ✅ **企業級多帳號解決方案** - 適合商業用戶
- ✅ **高級API支援** - 更多高級介面調用能力

</td>
<td width="30%" align="center">
<br>
<h3>立即行動</h3>
<a href="https://book.stripe.com/00w6oHbPkc7D1rn0iJ0Jq06">
  <img src="https://img.shields.io/badge/Stripe-支付-635BFF?style=for-the-badge&logo=stripe&logoColor=white" />
</a>
<br><br>
<p>安全支付 · 即時開通</p>
</td>
</tr>
</table>

> 💡 **提示**: 完成贊助後，請保留交易號並聯繫我們獲取贊助者特權

---

## 📞 聯繫我們

如有任何問題或建議，歡迎通過以下方式聯繫我們：

- **GitHub Issues**：[提交問題](https://github.com/WeChatPadPro/WeChatPadPro/issues)
- **Telegram 群組**：[加入討論](https://t.me/+LK0JuqLxjmk0ZjRh)
- **知識星球**：[深度交流](https://t.zsxq.com/Ygl6l)

---

<div align="center">
  <br>
  <p>
  <b>感謝您的支持和鼓勵！</b><br>
  WeChatPadPro 團隊
  </p>
  <br>
</div>

---

# 使用指南與注意事項

## ⚠️ 重要提醒

> **新帳號使用建議**
> - 建議新號穩定掛機3天後再使用高風險API操作
> - 請仔細閱讀下方[關於風控](#關於風控)章節的重要說明

### 登入注意事項

1. **異地登入處理**
   - 必須設置同城市的Socks5代理
   - 代理格式: `socks5://用戶名:密碼@代理IP:代理端口`
   - 代理優先級: 同城市IP > 同省IP
   - 家庭內網穿透socks5代理IP穩定性最佳
   - 需要搭建內網穿透socks5代理可聯繫我們協助
   > 推薦工具: [frp](https://github.com/fatedier/frp/releases)

2. **首次登入說明**
   - 可能出現立即掉線情況,重新掃碼登入2次後即可穩定
   - 24小時內可能會再次掉線(見下圖),使用原API `key`重新登入即可
   - 重新登入後一般可穩定使用3個月
   - 3天後基本穩定,7天後更穩定
   > ⚠️ 注意: 一個授權碼`key`僅限一個 WeChat 號使用,多帳號需生成多個授權碼

![登出錯誤示例](./static/doc/logout_error.png)

## 🛠️ 環境配置

### 基礎環境要求

- MySQL (推薦5.7及以上版本)
  - 資料庫: `wechat_mmtls`
  - 用戶名: `wechat_mmtls`
  - 密碼: `12345678`
- Redis
  - 密碼: `12345678`

> 對於MySQL 5.7以下版本,需提前使用[wechat_mmtls.sql](./wechat_mmtls.sql)建立資料庫表

### MySQL配置說明

如遇到以下錯誤:

![MySQL錯誤示例](./static/doc/error_mysql.png)

MySQL 5.7版本可通過修改配置解決:

```ini
[mysqld]
innodb_file_format = Barracuda
innodb_file_per_table = 1
innodb_large_prefix = 1
```

### 安裝指南

#### MySQL綠色版安裝

適用於所有作業系統,下載地址: https://downloads.mysql.com/archives/community/

**MySQL 5.7及以上版本**:
```shell
# 隨機生成root密碼初始化
mysqld --initialize --console

# 不設置root密碼初始化
mysqld --initialize-insecure
```

**MySQL 5.6及以下版本**:
```shell
# 1. 使用mysql_install_db初始化資料目錄
mysql_install_db --datadir="/path/to/your/mysql/data"
# 2. 啟動MySQL服務
mysqld --console
# 3. 設置root密碼
mysqladmin -u root password "你的root密碼"
# 4. 驗證登入
mysql -u root -p
```

> 如果bin目錄沒有mysql_install_db,需要使用Perl環境執行scripts/mysql_install_db.pl:
> - Linux/Mac自帶Perl
> - Windows需下載[Strawberry Perl](https://strawberryperl.com/)

#### Windows安裝

1. 下載對應版本MSI安裝包:
   - [MySQL 5.6.51](https://downloads.mysql.com/archives/get/p/25/file/mysql-installer-community-5.6.51.0.msi)
   - [MySQL 5.5.60](https://downloads.mysql.com/archives/get/p/25/file/mysql-installer-community-5.5.60.1.msi)
   - [MySQL 5.7.44](https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.44.0.msi)
   - [MySQL 8.0.39](https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-8.0.39.0.msi)

> 注意:
> - 以上均為32位版本,最大支援4GB記憶體
> - 64位版本請使用綠色版手動安裝
> - 安裝時選擇"自定義安裝",僅安裝MySQL-Server
> - 請記住設置的root密碼

![MySQL安裝示意圖](https://i0.hdslb.com/bfs/article/8591caf3b409951901930a5e1f4c25be495082007.png)

#### Linux安裝

推薦使用[寶塔面板](https://www.bt.cn/new/product_linux.html)進行安裝:
- 按提示選擇合適的MySQL版本
- 配置較低的伺服器建議使用低版本MySQL
- MySQL 5.7以下版本需提前導入[wechat_mmtls.sql](./wechat_mmtls.sql)

## ⚙️ 軟體配置

### 配置檔案說明

1. **setting.json**
   - debug: 是否開啟debug日誌
   - port: 服務端口號
   - apiVersion: API版本(如/v849)
   - ghWxid: 引流關注的公眾號wxid
   - adminKey: 管理介面授權KEY
   - redisConfig:
     - Port: Redis端口
     - Db: Redis資料庫號
     - Pass: Redis密碼
   - mySqlConnectStr: MySQL連接字串

2. **owner.json**
   ```json
   {
     "wxid_xxx": 1
   }
   ```
   > 設置管理員wxid後可通過檔案傳輸助手使用管理命令

## 🚀 啟動流程

1. 修改配置檔案
   - 設置setting.json中的adminKey和資料庫連接資訊
   - 在owner.json中新增管理員wxid

2. 初始化資料庫(MySQL 5.7+)
   - Linux: `./01_InitMySQL -passwd="root密碼"`
   - Windows: `01_InitMySQL.exe -passwd="root密碼"`

3. 啟動服務
   - Linux: `/opt/wechat/wechat_service >/opt/wechat/run.log 2>&1 &`
   - Windows: 雙擊`wechat_service.exe`

4. 獲取AuthKey
   訪問: `http://127.0.0.1:8848/v849/login/GenAuthKey2?key=ADMIN_KEY&count=1&days=365`

5. 登入操作
   - 獲取二維碼(需配置本地代理)
   - 監控二維碼狀態

## 🔒 關於風控

### 一、登入掉線問題

#### 1. 掉線情況說明

新用戶首次登入 API 平台或通過 iPad 協議登入 WeChat，**24 小時內首次登入必定掉線一次**。掉線後使用 `fwcid` 調用二維碼掃碼登入介面，即可實現 3 個月持續在線。

#### 2. 掉線原因分析

- **登入地異常**：登入地與歷史記錄差異過大
- **混用組件**：本地組件/`ttuid`切換、代理變動頻繁
- **頻繁登入/登出**：60 秒內重複掃碼或獲取`fwcid`
- **裝置重裝或更換**：更換裝置登入
- **長期離線**：裝置連續兩天不活躍
- **多裝置並行**：多台裝置同時登入且頻繁切換

#### 3. 建議對策

- 使用固定裝置與IP網路
- 登入後保持活躍1-2天再進行高頻操作
- 避免頻繁更換裝置或重裝應用

### 二、介面風控問題

#### 1. 高風控敏感操作

| 功能介面 | 延遲啟用時間 | 限制來源 |
|---------|------------|----------|
| 獲取群二維碼 | 3 天 | 官方限制 |
| 發起群聊 | 1 天 | 官方限制 |
| 建立群聊 | 3 天 | 平台限制 |
| 新增好友 | 7 天 | 平台限制 |
| 自動新增好友 | 7 天 | 平台限制 |

#### 2. 安全環境判定

- 登入後**48小時內為非安全期**
- 使用`fwcid`登入並保持活躍超過24小時後視為安全環境

### 三、風控等級與申訴處理

| 等級 | 表現形式 | 封禁時長 | 可否申訴 | 成功率 |
|------|---------|----------|---------|--------|
| 1級 | 操作過於頻繁 | 1天 | 否 | - |
| 2級 | 功能封鎖 | 7天 | 是 | 約30% |
| 3級 | 限制登入 | 30天 | 是 | 約15% |
| 4級 | 永久封鎖 | 永久 | 極難 | <10% |

### 四、防範建議彙總

- **登入與裝置管理**
  - 固定IP、固定裝置
  - 新裝置登入後靜置24小時
  
- **操作行為管控**
  - 控制好友新增數、拉群次數
  - 避免發送違規內容
  
- **內容合規與風險提示**
  - 使用內容審查工具過濾文案
  - 禁用第三方外掛或工具模擬登入
  
- **提升帳號權重**
  - 完成WeChat實名認證
  - 經常互動增加信任度

### 五、技術支援

如遇以下情況請聯繫技術支援：

- 大量帳號觸發風控
- 介面無回應/無報錯但操作失敗
- 接入長期自動化專案需穩定API支援
- 合規審核與企業授權申請協助

## 🧪 測試指南

推薦使用[ApiPOST經典版(v7.2.X)](https://www.apipost.cn/download.html):

1. 導入[WeChat849.apipost.v7.json](./static/swagger/WeChat849.apipost.v7.json)
2. 設置環境變數:
   - WS_URL: WebSocket基礎URL
   - ADMIN_KEY: 管理介面KEY
   - SOCKS5: 本地代理地址
3. 調用介面獲取TOKEN_KEY

![ApiPOST設置1](./static/doc/apipost1.png)

![ApiPOST設置2](./static/doc/apipost2.png)

