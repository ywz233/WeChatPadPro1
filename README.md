# WeChatPadPro

## 项目简介
WeChatPadPro:基於WeChat Pad協議的高級管理工具一个跨平台应用程序，支持Windows、MacOS、Linux、FreeBSD和OpenBSD等多种操作系统和架构。

## 支持平台
- Windows PC (64-bit)
- Windows ARM64设备
- Mac (Intel CPU)
- Mac (M1/M2 CPU)
- Linux (多种架构: amd64, arm64, mips64, mips64le, ppc64, ppc64le, riscv64)
- FreeBSD (amd64, arm64)
- OpenBSD (amd64, arm64)

## 安装说明

1. 下载对应您系统的压缩包：`wechatpadpro_v{版本号}_{构建日期}_{平台}.zip`
2. 解压缩文件到任意目录
3. 配置环境变量(可选)

## 配置说明

1. 在程序目录中复制`.env.example`文件并重命名为`.env`
2. 根据需要修改`.env`文件中的配置项
3. 根据需要修改`webhook_config.json`配置文件

## 运行方法

### Windows系统
```
# 在命令提示符或PowerShell中
cd 程序目录路径
.\wechatpadpro-windows-amd64-v版本号.exe
```

### MacOS系统
```
# 在终端中
cd 程序目录路径
chmod +x ./wechatpadpro-macos-amd64-v版本号  # 或 wechatpadpro-macos-arm64-v版本号
./wechatpadpro-macos-amd64-v版本号  # 或 ./wechatpadpro-macos-arm64-v版本号
```

### Linux/FreeBSD/OpenBSD系统
```
# 在终端中
cd 程序目录路径
chmod +x ./wechatpadpro-linux-amd64-v版本号  # 根据您的平台和架构调整文件名
./wechatpadpro-linux-amd64-v版本号  # 根据您的平台和架构调整文件名
```

## 常见问题

如遇到权限问题，请确保二进制文件具有执行权限：
```
chmod +x ./wechatpadpro-*
```

## 版本信息

当前版本: ios18.61-861

每个发行版中包含`version.txt`文件，其中包含详细的构建信息和平台支持情况。 