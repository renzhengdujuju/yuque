---
title: "Redis环境搭建"
yuque_slug: "vmpiormu0fd25758"
yuque_id: 270154123
yuque_updated: "2026-05-22T03:53:50.000Z"
source: "https://www.yuque.com/sunny-rzi3j/gghhdb/vmpiormu0fd25758"
---
## Redis 安装

Redis 在 Rocky Linux release 9.4 (Blue Onyx) 通过 YUM 安装 redis-6.2.19-1.el9\_6.x86\_64

```shell
[root@localhost ~]# cat /etc/redhat-release
Rocky Linux release 9.4 (Blue Onyx)
[root@localhost ~]# yum provides redis
Last metadata expiration check: 0:06:10 ago on Sat Aug  2 12:50:25 2025.
redis-6.2.19-1.el9_6.x86_64 : A persistent key-value database
Repo        : appstream
Matched from:
Provide    : redis = 6.2.19-1.el9_6
[root@localhost ~]# yum install redis-6.2.19-1.el9_6 -y
```

## Redis 启动和连接

### Redis 服务启动

**使用 systemd 管理**

```shell
# 服务文件
[root@localhost ~]# ls -l /usr/lib/systemd/system/redis.service
# 重新加载 systemd
systemctl daemon-reload
# 启动 Redis 服务
systemctl start redis
# 设置开机自启
systemctl enable redis
# 查看服务状态
systemctl status redis
```

**手动启动**

```shell
# 前台启动（用于调试）
redis-server /etc/redis/redis.conf
# 后台启动
redis-server /etc/redis/redis.conf --daemonize yes
# 指定端口启动
redis-server --port 6380
# 指定配置参数启动
redis-server --maxmemory 1gb --maxmemory-policy allkeys-lru
```

### Redis 客户端连接

**本地连接**

```shell
# 默认连接
redis-cli
# 指定主机和端口
redis-cli -h 127.0.0.1 -p 6379
# 使用密码连接
redis-cli -h 127.0.0.1 -p 6379 -a your_password
# 连接后认证
redis-cli
127.0.0.1:6379> AUTH your_password
OK
# 选择数据库
127.0.0.1:6379> SELECT 1
OK
127.0.0.1:6379[1]>
```

**远程连接配置**

```shell
# 服务器端配置
# 修改配置文件
vim /etc/redis/redis.conf
# 修改绑定地址 允许所有IP访问（生产环境需配合防火墙使用）
bind 0.0.0.0
# 设置强密码：防止未授权访问
requirepass your_strong_password
# 重启服务
systemctl restart redis

# 客户端连接
# 远程连接
redis-cli -h 192.168.1.100 -p 6379 -a your_password
```

在命令行中直接使用 `-a` 带上明文密码，容易被系统的历史命令记录（如 `.bash_history`）泄露。更安全的做法是省略 `-a` 参数，直接执行 `redis-cli -h 192.168.1.100 -p 6379`，进入交互界面后再输入 `AUTH your_password` 进行认证。

连接成功后，你可以随时输入以下命令来验证网络和认证是否通畅：

192.168.1.100:637**9**> PING

PONG *# 返回 PONG 说明连接完全正常*

## Redis 配置

### 配置文件详解

Redis 的主配置文件通常位于 `/etc/redis/redis.conf`，包含了所有的配置选项。

**配置文件结构：**

```shell
# Redis 配置文件主要部分
1. 网络配置 (NETWORK)
2. 通用配置 (GENERAL)
3. 快照配置 (SNAPSHOTTING)
4. 复制配置 (REPLICATION)
5. 安全配置 (SECURITY)
6. 客户端配置 (CLIENTS)
7. 内存管理 (MEMORY MANAGEMENT)
8. 惰性释放 (LAZY FREEING)
9. 线程 I/O (THREADED I/O)
10. 内核透明大页 (KERNEL TRANSPARENT HUGEPAGE)
11. 追加模式 (APPEND ONLY MODE)
12. LUA 脚本 (LUA SCRIPTING)
13. Redis 集群 (REDIS CLUSTER)
14. 慢日志 (SLOW LOG)
15. 延迟监控 (LATENCY MONITOR)
16. 事件通知 (EVENT NOTIFICATION)
17. 高级配置 (ADVANCED CONFIG)
```

### 常用配置参数

**网络配置**

```shell
# 绑定地址
bind 127.0.0.1 ::1
# 允许所有地址访问（生产环境需谨慎）
# bind 0.0.0.0

# 端口号
port 6379

# TCP 监听队列长度
tcp-backlog 511

# 客户端空闲超时时间（秒）
timeout 0

# TCP keepalive
tcp-keepalive 300
```

**通用配置**

```shell
# 以守护进程方式运行  
系统后台一直默默运行、没人盯着、开机自动启动、关机才停止的程序）
daemonize yes

# 进程文件
pidfile /var/run/redis_6379.pid

# 日志级别：debug, verbose, notice, warning
loglevel notice

# 日志文件
logfile /var/log/redis/redis-server.log

# 数据库数量
databases 16

# 显示 Redis logo
always-show-logo no
```

**内存管理**

```shell
# 最大内存限制
maxmemory 2gb

# 内存淘汰策略
# noeviction: 不淘汰，内存满时报错
# allkeys-lru: 所有键 LRU 淘汰
# volatile-lru: 有过期时间的键 LRU 淘汰
# allkeys-random: 所有键随机淘汰
# volatile-random: 有过期时间的键随机淘汰
# volatile-ttl: 有过期时间的键按 TTL 淘汰
# allkeys-lfu: 所有键 LFU 淘汰
# volatile-lfu: 有过期时间的键 LFU 淘汰
maxmemory-policy allkeys-lru

# LRU 和 LFU 算法样本数量
maxmemory-samples 5
```

### 安全配置

**密码认证**

```shell
# 设置密码
requirepass your_strong_password_here

# 重命名危险命令
  #将清空当前数据库的命令重命名为空字符串（即彻底禁用）
rename-command FLUSHDB ""
  #将清空所有数据库的命令彻底禁用
rename-command FLUSHALL ""
  #禁用全量键扫描命令
rename-command KEYS ""
  #将动态修改配置的 CONFIG 命令重命名为一个极难猜测的复杂字符串。
rename-command CONFIG "CONFIG_9a8b7c6d5e4f"
```

**注意事项**

1. **重启生效**：修改完 `redis.conf` 配置文件后，必须执行 `systemctl restart redis` 重启服务，新的命令规则才会正式生效。
2. **AOF 持久化的兼容性**：如果你开启了 AOF 持久化模式，在重命名某些特定命令（尤其是 `FLUSHALL`）时，可能会导致 Redis 无法正常启动或主从复制出现问题。如果遇到这种情况，建议在业务上线初期就做好重命名规划，或者在处理 AOF 文件时格外小心。

**ACL 用户管理**

```shell
# 启用 ACL 日志
acllog-max-len 128

# ACL 配置文件
# aclfile /etc/redis/users.acl

# 示例 ACL 配置
# user default on nopass ~* &* -@all +@read +@write
# user app_user on >app_password ~app:* +@read +@write -@dangerous
# user readonly_user on >readonly_password ~* +@read -@write -@dangerous
```

**网络安全**

```shell
# 保护模式（默认开启）
protected-mode yes

# 绑定到特定接口
bind 127.0.0.1 192.168.1.100

# 禁用某些命令
rename-command DEBUG ""
rename-command EVAL ""
rename-command SCRIPT ""
```

### 性能调优配置

**持久化优化**

通俗来说，RDB 就是给 Redis 内存里的数据**“拍快照”**。它会在指定的时间间隔内，把当前内存里所有的键值对数据，生成一个经过压缩的**二进制文件**（默认文件名是 `dump.rdb`）保存到硬盘上。当 Redis 意外宕机或重启时，它就会直接读取这个快照文件，把数据重新加载回内存中。

```shell
#RDB设置
save 900 1      # 900秒（15分钟）内至少有 1 个键发生变化，触发快照
save 300 10     # 300秒（5分钟）内至少有 10 个键发生变化，触发快照
save 60 10000   # 60秒（1分钟）内至少有 10000 个键发生变化，触发快照

# RDB 文件压缩
rdbcompression yes

# RDB 文件校验
rdbchecksum yes

# RDB 文件名
dbfilename dump.rdb

# 工作目录
dir /var/lib/redis

# AOF 配置
appendonly yes
appendfilename "appendonly.aof"

# AOF 同步策略
# always: 每个写操作都同步
# everysec: 每秒同步一次
# no: 由操作系统决定
appendfsync everysec

# AOF 重写配置
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
```

**AOF同步状态**

- `always`**（最安全，最慢）**：每次执行完写命令，就立刻把这条命令同步到硬盘。即使宕机也最多只丢一条数据，但频繁的磁盘 I/O 会严重拖慢 Redis 的性能。
- `everysec`**（推荐，平衡之选）**：**每秒同步一次**。这是默认也是生产环境最常用的策略。它在性能和安全性之间取得了很好的平衡，万一发生故障，最多只会丢失最近 1 秒钟的数据。
- `no`**（最快，最不安全）**：不主动控制同步频率，完全交给操作系统自己去决定什么时候刷盘。性能最好，但如果系统崩溃，可能会丢失大量数据。

**AOF重写**

因为 AOF 是不断追加记录的，时间久了文件会变得非常巨大，而且里面会有大量冗余命令（比如对同一个键连续修改了 100 次，其实只需要保留最后一次的值即可）。为了解决这个问题，Redis 引入了 **AOF 重写（Rewrite）** 机制。

当满足特定条件时，Redis 会在后台启动一个新进程，扫描当前内存里的所有数据，重新生成一个精简版的 AOF 文件（剔除无效和重复的命令）。

**AOF和RDB是什么？有什么区别？**

AOF 和 RDB 本质上就是 Redis 为了防止数据丢失，而采取的**两种不同的“存档”方式**。它们的需求和目标是一致的（都是为了把内存里的数据存到硬盘上），只是实现的思路完全不同。

为了让你印象更深刻，我们可以用一个非常经典的比喻来区分它们：

- **RDB（拍照片）**：它记录的是**结果**。就像每隔一段时间给整个数据库拍一张全景照（快照）。不管中间发生了什么，我只保存这一瞬间所有数据的最终状态。
- **AOF（记流水账/录视频）**：它记录的是**过程**。就像一个忠实的秘书，把你做的每一个写操作命令（比如 SET, DEL）都按顺序记在日记本里。

举个具体的例子：假设你对一个商品库存 `stock` 连续做了三次操作：

1. 初始是 100
2. 卖出 1 个 (`DECR stock`) -> 变成 99
3. 又卖出 1 个 (`DECR stock`) -> 变成 98
4. 补货 50 个 (`INCRBY stock 50`) -> 变成 148

面对这同样的操作，两者的记录形式截然不同：

- **如果是 RDB**：它根本不关心你卖了几次、补了几次货。到了触发时间点，它直接拍一张照，文件里只有一行冷冰冰的最终结果：`stock = 148`。
- **如果是 AOF**：它会老老实实地把你的操作过程全部记下来：`DECR stock`、`DECR stock`、`INCRBY stock 50`。重启时，Redis 就会像个机器人一样，把这些命令重新执行一遍，最终也能算出 148。

正因为这种本质上的区别，导致了它们在实战中各有优劣：

- **RDB（看结果）**：文件非常小（因为只存最终数据），恢复速度极快（直接把照片加载进内存就行）。但缺点是如果刚拍完照下一秒就断电，这期间的所有操作就全丢了。
- **AOF（看过程）**：数据非常安全（默认每秒记一次账，最多丢 1 秒的数据），而且日志是文本的，万一误删了库还能打开文件手动改回来。但缺点是文件体积大（废话太多），恢复速度慢（得一条条重放命令）。

**客户端连接优化**

```shell
# 最大客户端连接数
maxclients 10000

# 客户端输出缓冲区限制
# client-output-buffer-limit <class> <hard limit> <soft limit> <soft seconds>
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit replica 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60

# 客户端查询缓冲区限制
client-query-buffer-limit 1gb
```

**慢查询配置**

```shell
# 慢查询阈值（微秒）
slowlog-log-slower-than 10000

# 慢查询日志长度
slowlog-max-len 128
```

## 实践操作

### 需求描述

在生产环境中，我们经常需要在同一台服务器上运行多个Redis实例，用于不同的业务场景或实现数据隔离。本实践将演示如何通过自定义配置文件部署多个Redis实例，包括主实例、缓存实例和会话实例。

### 实践细节和结果验证

在 Redis 的配置文件里，路径的指定规则分为两派：

- **第一派：“独行侠” (绝对路径)**

- `pidfile` 和 `logfile` 就属于这一派。
- 你看它们的配置：`pidfile /var/run/redis/redis-main.pid`、`logfile /var/log/redis/redis-main.log`。
- 它们后面跟的都是**完整的绝对路径**（从根目录 `/` 开始写起）。因为已经写死了完整的路径，所以它们根本不看 `dir` 的脸色，爱存哪里存哪里。

- **第二派：“依赖者” (相对路径)**

- `dbfilename` 就属于这一派。
- 你看它的配置：`dbfilename dump-main.rdb`。它只给了一个光秃秃的文件名，没有带任何斜杠 `/`。
- 这时候它就懵了：“我该住哪儿？” 于是它必须去求助老大哥 `dir`。`dir` 说：“来我的地盘吧”，于是它就被放进了 `/var/lib/redis/main/` 里面。

```shell
# 1. 创建多实例目录结构
mkdir -p /etc/redis/instances/{main,cache,session}
mkdir -p /var/lib/redis/{main,cache,session}
mkdir -p /var/log/redis

# 2. 创建主实例配置文件 (端口6380)
tee /etc/redis/instances/main/redis.conf > /dev/null << 'EOF'
# Redis 主实例配置 - 用于核心业务数据
port 6380
bind 127.0.0.1
daemonize yes
pidfile /var/run/redis/redis-main.pid
logfile /var/log/redis/redis-main.log
dir /var/lib/redis/main  
dbfilename dump-main.rdb
#dir 是 Redis 配置文件里的一个专属参数，它的含义是“Directory”（目录），作用是告诉 Redis 
程序：“你以后就把数据文件（比如快照、日志）存放在这个指定的路径里”。

# 内存配置
maxmemory 1gb
maxmemory-policy allkeys-lru

# 持久化配置
save 900 1
save 300 10
save 60 10000
appendonly yes
appendfilename "appendonly-main.aof"

# 安全配置
requirepass main_redis_2025
rename-command FLUSHDB ""
rename-command FLUSHALL ""

# 客户端配置
maxclients 1000
timeout 300
EOF

# 3. 创建缓存实例配置文件 (端口6381)
sudo tee /etc/redis/instances/cache/redis.conf > /dev/null << 'EOF'
# Redis 缓存实例配置 - 用于应用缓存
port 6381
bind 127.0.0.1
daemonize yes
pidfile /var/run/redis/redis-cache.pid
logfile /var/log/redis/redis-cache.log
dir /var/lib/redis/cache
dbfilename dump-cache.rdb

# 内存配置 - 缓存实例分配更多内存
maxmemory 2gb
maxmemory-policy allkeys-lru
maxmemory-samples 10

# 持久化配置 - 缓存数据可以不持久化
save ""
appendonly no

# 安全配置
requirepass cache_redis_2025

# 客户端配置
maxclients 2000
timeout 0

# 过期键删除配置
hz 10
EOF

# 4. 创建会话实例配置文件 (端口6382)
sudo tee /etc/redis/instances/session/redis.conf > /dev/null << 'EOF'
# Redis 会话实例配置 - 用于用户会话存储
port 6382
bind 127.0.0.1
daemonize yes
pidfile /var/run/redis/redis-session.pid
logfile /var/log/redis/redis-session.log
dir /var/lib/redis/session
dbfilename dump-session.rdb

# 内存配置
maxmemory 512mb
maxmemory-policy volatile-lru

# 持久化配置 - 会话数据需要持久化但频率可以低一些
save 1800 1
save 300 100
appendonly yes
appendfilename "appendonly-session.aof"
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb

# 安全配置
requirepass session_redis_2025

# 客户端配置
maxclients 500
timeout 1800

# 键空间通知 - 用于会话过期监控
notify-keyspace-events Ex
EOF

# 5. 创建运行时目录
mkdir -p /var/run/redis
chown redis:redis /var/run/redis
chown -R redis:redis /var/lib/redis
chown -R redis:redis /var/log/redis
chown -R redis:redis /etc/redis/instances

# 6. 创建systemd服务文件
# 主实例服务
tee /etc/systemd/system/redis-main.service > /dev/null << 'EOF'
[Unit]
Description=Redis Main Instance
After=network.target

[Service]
Type=notify
ExecStart=/usr/bin/redis-server /etc/redis/instances/main/redis.conf --supervised systemd
ExecStop=/usr/bin/redis-cli -p 6380 -a main_redis_2025 shutdown
TimeoutStopSec=0
Restart=always
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
EOF

# 缓存实例服务
sudo tee /etc/systemd/system/redis-cache.service > /dev/null << 'EOF'
[Unit]
Description=Redis Cache Instance
After=network.target

[Service]
Type=notify
ExecStart=/usr/bin/redis-server /etc/redis/instances/cache/redis.conf --supervised systemd
ExecStop=/usr/bin/redis-cli -p 6381 -a cache_redis_2025 shutdown
TimeoutStopSec=0
Restart=always
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
EOF

# 会话实例服务
sudo tee /etc/systemd/system/redis-session.service > /dev/null << 'EOF'
[Unit]
Description=Redis Session Instance
After=network.target

[Service]
Type=notify
ExecStart=/usr/bin/redis-server /etc/redis/instances/session/redis.conf --supervised systemd
ExecStop=/usr/bin/redis-cli -p 6382 -a session_redis_2025 shutdown
TimeoutStopSec=0
Restart=always
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
EOF

# 7. 重新加载systemd并启动服务
sudo systemctl daemon-reload
sudo systemctl enable redis-main redis-cache redis-session
sudo systemctl start redis-main redis-cache redis-session

# 检查服务状态
sudo systemctl status redis-main redis-cache redis-session --no-pager

# 检查端口监听
sudo netstat -tlnp | grep redis-server

# 检查进程
ps aux | grep redis-server | grep -v grep

# 测试主实例
redis-cli -p 6380 -a main_redis_2025 ping
redis-cli -p 6380 -a main_redis_2025 set main:test "Main Instance Data"
redis-cli -p 6380 -a main_redis_2025 get main:test

# 测试缓存实例
redis-cli -p 6381 -a cache_redis_2025 ping
redis-cli -p 6381 -a cache_redis_2025 set cache:user:1001 "User Cache Data" EX 3600
redis-cli -p 6381 -a cache_redis_2025 get cache:user:1001
redis-cli -p 6381 -a cache_redis_2025 ttl cache:user:1001

# 测试会话实例
redis-cli -p 6382 -a session_redis_2025 ping
redis-cli -p 6382 -a session_redis_2025 set session:user:1001 "User Session Data" EX 1800
redis-cli -p 6382 -a session_redis_2025 get session:user:1001
redis-cli -p 6382 -a session_redis_2025 ttl session:user:1001

# 检查内存使用情况 - used_memory_human
redis-cli -p 6380 -a main_redis_2025 info memory | grep used_memory_human
redis-cli -p 6381 -a cache_redis_2025 info memory | grep used_memory_human
redis-cli -p 6382 -a session_redis_2025 info memory | grep used_memory_human

# 检查配置信息 - maxmemory
redis-cli -p 6380 -a main_redis_2025 config get maxmemory
redis-cli -p 6381 -a cache_redis_2025 config get maxmemory
redis-cli -p 6382 -a session_redis_2025 config get maxmemory
```

### [Unit] 基础信息区
- `Description=Redis Main Instance`：给这个服务起个名字，方便你在查看系统服务列表时一眼认出它。
- `After=network.target`：告诉 Systemd，一定要等系统的网络功能准备好之后，再启动 Redis。因为 Redis 是一个网络数据库，没有网络它是没法正常工作的。

### [Service] 核心运行区（最关键的部分）
- `Type=notify`：这是一种高级的进程通知模式。配合后面 `ExecStart` 里的 `--supervised systemd` 参数，Redis 启动成功后会主动向 Systemd “汇报一声”，让系统精准地知道它已经准备就绪了。
- `ExecStart=/usr/bin/redis-server /etc/redis/instances/main/redis.conf --supervised systemd`：这是真正的启动命令。它告诉系统去调用 Redis 服务端程序，**加载你之前写好的主实例配置文件**（`/etc/redis/instances/main/redis.conf`），并开启 Systemd 监管模式。
- `ExecStop=/usr/bin/redis-cli -p 6380 -a main_redis_2025 shutdown`：这是停止服务的命令。当你执行停止指令时，系统会通过 Redis 客户端连接 6380 端口，输入密码 `main_redis_2025`，然后优雅地发送关闭指令。
- `TimeoutStopSec=0`：在停止服务时不设超时限制。这能确保即使 Redis 当时正在处理大量数据或进行持久化操作，也能有足够的时间安全退出，避免数据损坏。
- `Restart=always`：**生产环境的保命配置！** 只要 Redis 进程意外崩溃或被误杀，Systemd 都会无条件地立刻把它重新拉起来，保证服务的高可用性。
- `User=redis`**/**`Group=redis`：再次强调安全原则，强制使用专用的普通用户 `redis` 来运行这个服务，而不是用最高权限的 root。
- `RuntimeDirectory=redis`：让 Systemd 自动在 `/var/run` 下为 Redis 创建运行时目录（对应你之前手动创建的 `/var/run/redis`），并设置好对应的权限（`0755`）。这样每次服务器重启后，Redis 都能顺利找到地方存放 PID 文件。

### [Install] 安装挂载区
- `WantedBy=multi-user.target`：意思是当服务器进入正常的多用户运行模式（也就是大家平时最常用的命令行或图形界面模式）时，这个 Redis 服务应该被自动激活。配合 `systemctl enable` 命令，就能实现开机自启。
