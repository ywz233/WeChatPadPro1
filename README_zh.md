# WeChatPadPro:基於WeChat Pad協議的高級管理工具

## 📢 重要！老用戶必讀

> **如果您是長期使用WeChatPadPro的用戶，請先閱讀本節內容！**

為避免首次啓動或長時間未使用後啓動時收到大量過期消息，我們強烈建議：

1. 先運行 `start_webhook_client.bat`(Windows) 或 `start_webhook_client.sh`(MacOS/Linux) 啓動消息緩存服務
2. 然後再啓動主程序

**詳細教程：** 請參閱 [`usage_guide_for_old_users.md`](usage_guide_for_old_users.md) 文件

---

## 項目簡介
WeChatPadPro是一個跨平臺應用程序，支持Windows、MacOS、Linux、FreeBSD和OpenBSD等多種操作系統和架構。

## 支持平臺
- Windows PC (64-bit)
- Windows ARM64設備
- Mac (Intel CPU)
- Mac (M1/M2 CPU)
- Linux (多種架構: amd64, arm64, mips64, mips64le, ppc64, ppc64le, riscv64)
- FreeBSD (amd64, arm64)
- OpenBSD (amd64, arm64)

## 安裝說明

1. 下載對應您系統的壓縮包：`wechatpadpro_v{版本號}_{構建日期}_{平臺}.zip`
2. 解壓縮文件到任意目錄
3. 配置環境變量(可選)

## 配置說明

1. 在程序目錄中複製`.env.example`文件並重命名為`.env`
2. 根據需要修改`.env`文件中的配置項
3. 根據需要修改`webhook_config.json`配置文件

## 運行方法

### 🔴 老用戶推薦啓動順序

1. 運行 webhook 客戶端（避免大量消息堆積）
   - Windows: 雙擊 `start_webhook_client.bat`
   - MacOS/Linux: 執行 `./start_webhook_client.sh`

2. 啓動主程序
   - Windows: 雙擊 `run.bat`
   - MacOS/Linux: 執行 `./run.sh`

### Windows系統
```
# 在命令提示符或PowerShell中
cd 程序目錄路徑
.\run.bat
```

### MacOS/Linux/BSD系統
```
# 在終端中
cd 程序目錄路徑
chmod +x ./run.sh
./run.sh
```

## 常見問題

如遇到權限問題，請確保二進制文件具有執行權限：
```
chmod +x ./wechatpadpro-*
```

## 版本信息

當前版本: ios18.61-861
構建時間: 2025-07-18

每個發行版中包含`version.txt`文件，其中包含詳細的構建信息和平台支持情況。 