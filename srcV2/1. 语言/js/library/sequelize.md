#### 1. 快速入门



##### 1.1  安装

```
// 通过 npm 安装
npm install --save sequelize
```

- 你还需要手动安装对应的数据库驱动程序:

```
# 选择对应的安装:
$ npm install --save pg pg-hstore # Postgres
$ npm install --save mysql2
$ npm install --save mariadb
$ npm install --save sqlite3
$ npm install --save tedious # Microsoft SQL Server
```

##### 1.2 建立连接

- 要连接到数据库,你必须创建 Sequelize 实例.
  - 通过将连接参数分别传递给 Sequelize 构造函数或传递单个连接 URI 来完成

```javascript
const Sequelize = require('sequelize');

//方法1:单独传递参数
const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: /* 'mysql' | 'mariadb' | 'postgres' | 'mssql' 之一 */
});

// 方法2: 传递连接 URI
const sequelize = new Sequelize('postgres://user:pass@example.com:5432/dbname');
```

- 连接池

  如果从单个进程连接到数据库,则应仅创建一个 Sequelize 实例. Sequelize 将在初始化时设置连接池. 可以通过构造函数的 `options` 参数(使用`options.pool`)配置此连接池,如以下示例所示:

  ```
  const sequelize = new Sequelize(/* ... */, {
    // ...
    pool: {
      max: 5,
      min: 0,
      acquire: 30000,
      idle: 10000
    }
  });
  ```

  在[Sequelize构造函数API参考](http://docs.sequelizejs.com/class/lib/sequelize.js~Sequelize.html#instance-constructor-constructor)中了解更多信息. **如果从多个进程连接到数据库,则必须为每个进程创建一个实例**,但每个实例应具有最大连接池大小,以便遵守总的最大大小.例如,如果你希望最大连接池大小为 90 并且你有三个进程,则每个进程的 Sequelize 实例的最大连接池大小应为 30.

##### 1.3 测试连接

你可以使用 `.authenticate()` 函数来测试连接.

```javascript
sequelize
  .authenticate()
  .then(() => {
    console.log('Connection has been established successfully.');
  })
  .catch(err => {
    console.error('Unable to connect to the database:', err);
  });
```



##### 1. 4 关闭连接

Sequelize 将默认保持连接持续,并对所有查询使用相同的连接. 如果需要关闭连接,请调用`sequelize.close()`(这是异步的并返回Promise).



##### 1.5 表建模

