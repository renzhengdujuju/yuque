---
title: "MongoDB安全管理"
yuque_slug: "ppsu29gawg5btl36"
yuque_id: 270126518
yuque_updated: "2026-05-17T03:38:26.000Z"
source: "https://www.yuque.com/sunny-rzi3j/gghhdb/ppsu29gawg5btl36"
---
确保数据库的安全是任何应用都必须考虑的关键问题。MongoDB 提供了丰富的安全特性，包括认证、授权、加密等，以保护数据免受未经授权的访问。本章节将详细介绍如何配置和管理 MongoDB 的各项安全功能。

---

## 安全概述

MongoDB 的安全模型主要围绕以下几个核心概念构建：

- **认证 (Authentication)**: 验证用户身份，确认“你是谁”。
- **授权 (Authorization)**: 控制经过认证的用户可以执行哪些操作，确认“你能做什么”。
- **加密 (Encryption)**: 保护数据在传输过程（TLS/SSL）和静态存储时（Encryption at Rest）的安全。
- **审计 (Auditing)**: 记录数据库上发生的活动，以便进行安全分析和合规性检查。

默认情况下，MongoDB 的安全特性是**未启用**的。必须显式地配置和启用它们。

---

## 认证机制 (Authentication)

启用认证是保护数据库的第一步。当认证启用后，所有客户端和数据库节点之间的连接都必须提供有效的凭据。

### 启用认证

在 `mongod.conf` 配置文件中或通过命令行参数启用认证：

```yaml
# mongod.conf
security:
  authorization: enabled
```

或者

```bash
mongod --auth
```

### 认证方法

MongoDB 支持多种认证机制，最常用的是 **SCRAM (Salted Challenge Response Authentication Mechanism)**。

- **SCRAM-SHA-1**: 默认机制。
- **SCRAM-SHA-256**: 更强的加密算法，建议在 MongoDB 4.0 及以上版本中使用。

### 创建管理员用户

在启用认证之前，必须先创建一个具有 `userAdminAnyDatabase` 角色的用户管理员。这个用户将用于创建和管理其他用户。

1. **以无认证模式启动**`mongod`。
2. **连接到**`admin`**数据库并创建用户**:

```javascript
use admin
db.createUser({
  user: "myUserAdmin",
  pwd: passwordPrompt(), // or a plain-text password
  roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
})
```

3. **重启**`mongod`**并启用认证**。

---

## 授权与基于角色的访问控制 (RBAC)

MongoDB 使用基于角色的访问控制（Role-Based Access Control, RBAC）来管理用户权限。权限被定义为**角色 (Role)**，然后将角色分配给用户。

### 内置角色 (Built-In Roles)

MongoDB 提供了一系列预定义的角色，涵盖了常见的管理和操作需求。

- **数据库用户角色**: `read`, `readWrite`
- **数据库管理员角色**: `dbAdmin`, `dbOwner`, `userAdmin`
- **集群管理员角色**: `clusterAdmin`, `clusterManager`, `hostManager`
- **备份与恢复角色**: `backup`, `restore`
- **所有数据库角色**: `readAnyDatabase`, `readWriteAnyDatabase`, `userAdminAnyDatabase`, `dbAdminAnyDatabase`
- **超级用户角色**: `root` (拥有所有权限)

### 自定义角色 (Custom Roles)

如果内置角色无法满足精细化权限控制需求，可以创建自定义角色。

```javascript
use myAppDB
db.createRole({
  role: "salesDataViewer",
  privileges: [
    { resource: { db: "myAppDB", collection: "sales" }, actions: ["find"] }
  ],
  roles: []
})
```

### 管理用户和角色

- `db.createUser()`: 创建用户。
- `db.updateUser()`: 更新用户信息（如密码、角色）。
- `db.dropUser()`: 删除用户。
- `db.createRole()`: 创建角色。
- `db.grantRolesToUser()`: 为用户授予角色。
- `db.revokeRolesFromUser()`: 撤销用户的角色。

---

## 网络加密 (TLS/SSL)

为了保护数据在网络传输过程中的安全，防止窃听和中间人攻击，应该为 MongoDB 部署启用 TLS/SSL 加密。

### 配置 TLS/SSL

1. **获取 TLS/SSL 证书**: 可以使用自签名证书（用于内部测试）或从证书颁发机构 (CA) 获取证书。
2. **配置**`mongod`**和**`mongos`: 在配置文件中指定证书文件、私钥文件和 CA 文件。

```yaml
net:
  tls:
    mode: requireTLS
    certificateKeyFile: /path/to/mongodb.pem
    CAFile: /path/to/ca.pem
```

3. **客户端连接**: 客户端在连接时也需要指定 TLS/SSL 选项。

```shell
mongo --ssl --sslCAFile /path/to/ca.pem --sslPEMKeyFile /path/to/client.pem ...
```

---

## 静态数据加密 (Encryption at Rest)

对于高度敏感的数据，除了网络加密，还应该考虑对存储在磁盘上的数据文件进行加密。

- **WiredTiger 的原生加密**: MongoDB Enterprise 版本支持使用 WiredTiger 存储引擎的原生加密功能。它使用本地密钥管理器或第三方密钥管理服务（如 KMIP）来管理加密密钥。
- **文件系统/磁盘加密**: 也可以在操作系统层面或通过云服务商提供的功能（如 AWS KMS, Azure Key Vault）对存储设备进行加密。

---

## 实践操作

### 需求描述

构建一个完整的MongoDB安全管理环境，模拟企业级应用的安全需求。该场景需要配置认证机制、创建不同权限的用户角色，并验证安全策略的有效性。通过实际操作来理解MongoDB安全管理的核心概念和最佳实践。

### 实践细节和结果验证

```shell
# 1. 启用认证并创建管理员用户
# 首先以无认证模式启动MongoDB
mongod --dbpath /data/mongodb/ins11 --port 27117 --fork --logpath /data/mongodb/logs/ins11.log

# 连接到MongoDB并创建管理员用户
mongosh --port 27117
# 在mongosh中执行：
use admin
db.createUser({
  user: "admin",
  pwd: "ealgeslab123",
  roles: [{ role: "userAdminAnyDatabase", db: "admin" }]
})

# 验证管理员用户创建成功
db.getUsers()
# 预期结果：显示创建的admin用户信息

# 2. 重启MongoDB并启用认证
# 停止MongoDB服务
db.adminCommand("shutdown")

# 以认证模式重新启动
mongod --auth --dbpath /data/mongodb/ins11 --port 27117 --fork --logpath /data/mongodb/logs/ins11.log

# 使用管理员账户登录
mongosh --port 27117 -u "admin" -p "ealgeslab123" --authenticationDatabase "admin"

# 3. 创建业务数据库和用户
# 创建应用数据库
use ecommerce

# 创建具有读写权限的业务用户
db.createUser({
  user: "appUser",
  pwd: "appPassword123",
  roles: [{ role: "readWrite", db: "ecommerce" }]
})

# 验证业务用户创建成功
db.getUsers()
# 预期结果：显示appUser用户信息

# 4. 创建自定义角色
# 创建只允许查询和更新特定集合的自定义角色
db.createRole({
  role: "productManager",
  privileges: [
    {
      resource: { db: "ecommerce", collection: "products" },
      actions: ["find", "update"]
    }
  ],
  roles: []
})

# 创建使用自定义角色的用户
db.createUser({
  user: "productAdmin",
  pwd: "productPassword123",
  roles: [{ role: "productManager", db: "ecommerce" }]
})

# 验证自定义角色和用户
db.getRoles({ showPrivileges: true })
db.getUsers()
# 预期结果：显示productManager角色和productAdmin用户

# 5. 测试用户权限
# 使用业务用户连接
mongosh --port 27117 -u "appUser" -p "appPassword123" --authenticationDatabase "ecommerce"

# 测试读写权限
use ecommerce
db.products.insertOne({ name: "Laptop", price: 999, category: "Electronics" })
db.products.find()
db.products.updateOne({ name: "Laptop" }, { $set: { price: 899 } })
# 预期结果：所有操作成功执行

# 6. 测试自定义角色权限
# 使用自定义角色用户连接
mongosh --port 27017 -u "productAdmin" -p "productPassword123" --authenticationDatabase "ecommerce"

use ecommerce
# 测试允许的操作
db.products.find()
db.products.updateOne({ name: "Laptop" }, { $set: { description: "High-performance laptop" } })
# 预期结果：查询和更新操作成功

# 测试禁止的操作
db.products.insertOne({ name: "Mouse", price: 25 })
# 预期结果：操作失败，提示权限不足

db.products.deleteOne({ name: "Laptop" })
# 预期结果：操作失败，提示权限不足

# 8. 查看用户和角色信息
# 查看所有用户
use admin
db.system.users.find().pretty()

# 查看所有角色  #pretty是为了查询结果更美观 不在一排显示完全
db.system.roles.find().pretty()

# 查看当前用户权限
db.runCommand({ connectionStatus: 1 })
# 预期结果：显示当前连接的用户信息和权限

# 9. 权限管理操作
use ecommerce
# 为用户添加新角色
db.grantRolesToUser("appUser", [{ role: "dbAdmin", db: "ecommerce" }])

# 撤销用户角色
db.revokeRolesFromUser("appUser", [{ role: "dbAdmin", db: "ecommerce" }])

# 修改用户密码
db.updateUser("appUser", { pwd: "newPassword123" })

# 验证权限变更
db.getUser("appUser")
# 预期结果：显示用户的最新角色信息
```

**面试官**

### 1. 认证与授权的底层逻辑（必考题）
面试官非常喜欢问：“MongoDB 开启认证的正确流程是什么？为什么不能直接开启？”

- **核心考点**：你是否理解 `admin` 数据库的特殊性，以及“先建用户后开认证”的死锁逻辑。
- **参考话术**：“MongoDB 默认是无认证的。开启认证前，必须先以无认证模式启动，进入 `admin` 库创建一个拥有 `userAdminAnyDatabase` 权限的管理员账号。这是因为一旦开启 `--auth`，如果没有提前创建好管理员，外部就无法再连接进来创建任何用户了。创建完后再重启服务并加上 `--auth` 参数。”

### 2. 最小权限原则 (Least Privilege) 与 RBAC
面试官可能会给出一个业务场景：“我们的应用只需要读取报表数据，应该给它什么权限？能不能直接给 `root` 或 `dbOwner`？”

- **核心考点**：考察你的安全意识。是否知道滥用高级权限的风险，以及是否会使用自定义角色。
- **参考话术**：“绝对不能给生产环境的应用账号赋予 `root` 或 `dbOwner` 这种超级权限。应该遵循**最小权限原则**。如果是只读业务，就给 `read` 角色；如果需要增删改查，就给 `readWrite`。如果业务需求很特殊（比如只能更新特定集合的某些字段），我会通过 `createRole` 创建自定义角色，精确控制它的 `actions` 和资源范围，最大程度降低误操作或 SQL 注入带来的风险。”

### 3. 常见的安全漏洞与防护（进阶加分项）
面试官可能会问：“除了账号密码，你还做了哪些安全措施来保护 MongoDB？”

- **核心考点**：网络隔离、传输加密和防注入。
- **参考话术**：

- **网络绑定**：“在生产环境中，我绝不会把 MongoDB 绑定到 `0.0.0.0` 暴露给公网。我会修改 `bindIp` 只监听内网 IP，并配合防火墙/安全组设置白名单，只允许应用服务器访问。”
- **防注入**：“在使用 Node.js 或其他语言驱动时，要注意防止 NoSQL 注入。比如在查询时避免直接把前端传来的对象拼接到查询条件里，要强制指定数据类型。”
- **禁用脚本执行**：“为了防止恶意代码执行，我通常会禁用服务端的 JavaScript 引擎（如 `$where` 操作符），因为大部分业务根本用不到它。”

### 4. 生产环境的部署架构安全
面试官可能会问：“单节点的 MongoDB 在生产环境有什么安全隐患？高可用架构下怎么保证安全？”

- **核心考点**：副本集（Replica Set）的安全配置。
- **参考话术**：“单节点有单点故障风险。在生产环境我们通常使用副本集。在副本集中，不仅要开启客户端的认证，节点之间通信也需要配置 **KeyFile**。这个 KeyFile 就像节点之间的‘暗号’，只有持有相同 KeyFile 的 mongod 实例才能加入同一个副本集并进行内部数据同步，防止恶意节点伪装成副本混入集群。”

### 5. 索引与安全的关系（冷门但惊艳的考点）
有时候面试官会结合性能问你：“大量的数据查询会不会带来安全问题？”

- **核心考点**：拒绝服务攻击（DoS）。
- **参考话术**：“如果某个集合没有建立合适的索引，恶意的全表扫描查询会消耗大量的 CPU 和内存，导致数据库卡死，这其实也是一种隐性的安全问题（DoS 攻击）。所以我们不仅要管权限，还要通过慢查询日志监控异常的全表扫描行为，并及时建立索引来规避。”
