FROM redis:6

# 创建日志目录
RUN mkdir -p /var/log/redis && \
    chown redis:redis /var/log/redis

# 复制Redis配置文件
COPY redis.conf /usr/local/etc/redis/redis.conf

# 设置健康检查
HEALTHCHECK --interval=10s --timeout=5s --retries=5 \
  CMD redis-cli -a 123456 ping || exit 1

# 使用自定义配置文件启动Redis
CMD ["redis-server", "/usr/local/etc/redis/redis.conf"] 