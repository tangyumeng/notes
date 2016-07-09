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
- 不要选用字典里面的单词做密码。甚至"xfish98"也比较差，"duag98"比较好，没有用"fish"而是使用对应字符在键盘上左面的字符组合。可以使用"4sa7ya"而不是“4 score and 7 years ago”,因为后者更容易记忆。
- 使用防火墙
- 不要信任用户的输入、防御性编程
- 不要在互联网上传输原始数据，使用加密协议比如 `SSL` 或者 `SSH`

### 保持密码安全
#### 终端密码安全原则

- 终端连接 MySQL 时，不要使用显式密码,mysql> `mysql -u uname -ppasswd db_name ` 。相应的使用 mysql>`mysql -u uname -p db_name` ,根据终端提示输入密码。
- 把密码存放在一个可选的文件中，比如在 `*nix` 系统中，可以把密码存放在用户家目录下面 `.my.cnf`文件中 `[client]` 部分（segment）,比如：

  ```ruby
  [client]
  password=your_pass
  ```

  > 为了保证密码安全，该文件的访问权限应该设置得只能由当前用户访问,为了保证这点，可以把访问权限设置为 `400` 或 `600`。比如 shell> `chmod 600 .my.cnf`

- 把密码保存在 `MYSQL_PWD` 环境变量中

#### 密码安全注意事项

- 在 Unix 系统中，mysql 客户端把执行过的语句写入一个名字叫 .mysql_history 文件中，该文件保存在用户家目录下。 其中 `CREATE USER` 和 `ALTER USER` SQL 语句中的密码会已明文形式写入文件，所以该文件应该设置正确的访问权限，以防被其他用户获取，其中设置用户访问权限可以参考 `.my.cnf` 的权限设置。
