
# WeChatPadPro Docker 部署教程

## 1. 克隆项目
首先，您需要克隆 WeChatPadPro 项目到本地：

```bash
git clone https://github.com/WeChatPadPro/WeChatPadPro.git
```

## 2. 进入部署目录
进入项目中的 `deploy` 目录：

```bash
cd WeChatPadPro/deploy
```

## 3. 配置环境变量
在 `deploy` 目录下，您会看到一个 `.env` 文件。您需要根据自己的环境配置此文件，或者直接使用以下配置：

### 示例配置：
```ini
# MySQL配置
MYSQL_ROOT_PASSWORD=root123456
MYSQL_DATABASE=weixin
MYSQL_USER=weixin
MYSQL_PASSWORD=123456
MYSQL_PORT=3306

# Redis配置
REDIS_PASSWORD=123456
REDIS_PORT=6379

# WeChat Pad Pro配置
WECHAT_PORT=8080
DB_HOST=wechatpadpro_mysql
DB_PORT=3306
DB_DATABASE=weixin
DB_USERNAME=weixin
DB_PASSWORD=123456
REDIS_HOST=wechatpadpro_redis
REDIS_DB=0

# 是否开启调试模式（true: 开启，false: 关闭）
DEBUG=false

# 服务监听地址（0.0.0.0表示监听所有网卡）
HOST=0.0.0.0

# 服务端口号
PORT=1238

# API版本前缀（如 /v1, /v2）
API_VERSION=

# MCP服务端口（用于AI大模型集成服务）
MCP_PORT=8099

# 推广公众号微信ID（用于新用户首次登录时推广）
GH_WXID=

# 管理员密钥（建议使用复杂的随机字符串）
ADMIN_KEY=999222

# ==========================================
# Redis配置
# ==========================================
# Redis服务器地址
REDIS_HOST=192.168.198.128

# Redis端口
REDIS_PORT=6379

# Redis数据库编号
REDIS_DB=1

# Redis密码
REDIS_PASS=123456

# Redis最大空闲连接数
REDIS_MAX_IDLE=30

# Redis最大活动连接数
REDIS_MAX_ACTIVE=100

# Redis空闲连接超时时间（毫秒）
REDIS_IDLE_TIMEOUT=5000

# Redis连接最大生命周期（秒）
REDIS_MAX_CONN_LIFETIME=3600

# Redis连接超时时间（毫秒）
REDIS_CONNECT_TIMEOUT=5000

# Redis读取超时时间（毫秒）
REDIS_READ_TIMEOUT=10000

# Redis写入超时时间（毫秒）
REDIS_WRITE_TIMEOUT=10000
```

## 4. 启动服务
在 `deploy` 目录下，使用 Docker Compose 启动服务：

```bash
docker-compose up -d
```

这条命令会在后台启动所有相关的 Docker 服务，包括 `wechatpadpro`、`mysql` 和 `redis`。

## 5. 查看服务状态
要查看各个服务的运行状态，可以使用以下命令：

```bash
docker-compose ps
```

如果一切正常，您应该能看到 `wechatpadpro`、`mysql` 和 `redis` 容器正在运行。

## 6. 配置服务端口和网络
在 `docker-compose.yml` 文件中配置了服务的端口映射，确保您根据需要调整这些端口：

- **WeChatPadPro**: 映射到端口 `1238`
- **MySQL**: 映射到端口 `3306`
- **Redis**: 映射到端口 `6379`

您可以根据需要修改这些端口映射。

## 7. 访问 WeChatPadPro 服务
启动服务后，您可以通过访问 `http://<Your_Server_IP>:1238` 来使用 WeChatPadPro 服务。

如果您在本地运行，URL 将是：

```
http://localhost:1238
```

## 8. 配置 MySQL 和 Redis 容器
- `mysql` 服务使用了健康检查机制，确保数据库容器正常运行。
- `redis` 服务配置了连接密码，默认密码为 `123456`，并且也有健康检查。

## 9. 使用 Docker Compose 进行服务管理
您可以使用以下命令来管理 Docker 服务：

- **停止服务**：

```bash
docker-compose down
```

- **重新启动服务**：

```bash
docker-compose restart
```

## 10. 日志查看
如果遇到问题，可以通过查看服务日志来诊断问题：

```bash
docker-compose logs -f
```

这将会实时显示服务的日志，帮助您排查问题。

## 总结
通过以上步骤，您成功部署了 WeChatPadPro 服务，并且将其与 MySQL 和 Redis 服务配合使用。如果您遇到任何问题，检查日志文件，确保配置文件中各个设置项正确无误。
