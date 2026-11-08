---
title: "mysql的悲观锁和乐观锁"
yuque_slug: "dasrstpnxfqq7wl4"
yuque_id: 227932762
yuque_updated: "2026-10-14T04:54:03.000Z"
source: "https://www.yuque.com/sunny-rzi3j/hln8qy/dasrstpnxfqq7wl4"
---
### MySQL的悲观锁与乐观锁

#### 悲观锁（Pessimistic Locking）

**含义：**

- 假设会发生冲突，因此在操作数据之前对数据加锁，确保其他事务无法访问该数据。

**实现方式：**

- **排它锁（Exclusive Lock）：** 当事务在操作数据时把这部分数据进行锁定，直到操作完毕后再解锁。这将防止其他进程读取或修改表中的数据。
- `SELECT ... FOR UPDATE`**语句：** 在事务中使用这个语句可以对所选择的数据行进行加锁处理，使得这些数据行在当前事务提交前不能被其他事务修改。

**示例：**

```sql
-- 开始事务
START TRANSACTION;

-- 对account表中name为"Max"的记录加锁
SELECT * FROM account WHERE name = 'Max' FOR UPDATE;

-- 更新操作...
UPDATE account SET balance = balance - 100 WHERE name = 'Max';

-- 提交事务，释放锁
COMMIT;
```

#### 乐观锁（Optimistic Locking）

**含义：**

- 假设不会发生冲突，因此在操作数据时不加锁，而是在更新数据时进行校验。如果发现数据被其他事务修改，则会拒绝当前事务的修改，需重新尝试。

**实现方式：**

- **版本号机制：** 给表添加一个版本号字段，在更新数据时检查版本号是否一致。如果不一致则说明数据已被其他事务修改过。
- **时间戳机制：** 类似版本号机制，但是使用时间戳来代替版本号。
- **条件判断（业务字段）：** 使用业务逻辑中的字段来进行并发控制，比如库存数量等。

**示例：** 假设我们有一个`products`表，其中包含`id`, `stock`, 和 `version`字段。我们可以这样实现乐观锁：

```sql
-- 表设计
ALTER TABLE products ADD COLUMN version INT DEFAULT 0;

-- 更新逻辑
UPDATE products
SET stock = stock - 1, version = version + 1
WHERE id = 1001 AND version = 当前版本;
```

如果`UPDATE`语句影响的行数为0，这意味着另一个事务已经先一步更新了这条记录，我们需要根据业务逻辑决定是否重试更新。

#### 锁的选择

- **悲观锁**适用于写多读少的场景，能够有效防止并发冲突，但可能导致较高的锁竞争和死锁的可能性。
- **乐观锁**适用于读多写少的场景，并发性能好，但需要应用程序处理并发冲突，如重试逻辑。

#### 注意事项

- 使用悲观锁时要注意避免长时间持有锁以减少死锁的可能性。
- 乐观锁需要合理设置重试策略，以免出现无限重试的情况。
- 根据实际应用场景选择合适的锁机制，有时也可能结合两者使用
