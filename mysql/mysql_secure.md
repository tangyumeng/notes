## MySQL 安全问题

MySQL 为用户执行的所有连接、查询以及其他操作使用访问控制列表来保证安全。MySQL 客户端和服务器端可以使用SSL加密的连接来保证安全。

### 运行 MySQL 时，遵循以下原则：

- 不要授权给除了 MySQL root 帐号以外用户对 mysql 数据库中 user 表的访问权限
- 掌握 MySQL 访问权限系统工作的原理
  - 使用 `GRANT` 、`REVOKE` 来控制对 MySQL 的访问
  - 不要授予超出需要的权限
  - 不要授予对所有主机的访问权限

- 不要在 MySQL 中存储明文密码
  > 为了防止密码恢复可以使用 `彩虹表(reinbow tables)`,不要在普通密码上使用方法；通常选用一个字符串作为 `salt`,并且使用 `hash(hash(password)+salt)` 作为值存放在数据库中
