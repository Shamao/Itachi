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

模型是一个扩展 Sequelize.Model 的类. 模型可以用两种等效方式定义. 第一个是Sequelize.Model.init(属性,参数):

```javascript
const Model = Sequelize.Model;
class User extends Model {}
User.init({
  // 属性
  firstName: {
    type: Sequelize.STRING,
    allowNull: false
  },
  lastName: {
    type: Sequelize.STRING
    // allowNull 默认为 true
  }
}, {
  sequelize,
  modelName: 'user'
  // 参数
});
```

另一个是使用 `sequelize.define`:

```javascript
const User = sequelize.define('user', {
  // 属性
  firstName: {
    type: Sequelize.STRING,
    allowNull: false
  },
  lastName: {
    type: Sequelize.STRING
    // allowNull 默认为 true
  }
}, {
  // 参数
});
```

- 在内部, `sequelize.define` 调用 `Model.init`.

#### 1.6 将模型与数据库同步

如果你希望 Sequelize 根据你的模型定义自动创建表(或根据需要进行修改),你可以使用`sync`方法,如下所示:

```
// 注意:如果表已经存在,使用`force:true`将删除该表
User.sync({ force: true }).then(() => {
  // 现在数据库中的 `users` 表对应于模型定义
  return User.create({
    firstName: 'John',
    lastName: 'Hancock'
  });
});
```

#### 1.7 一次同步所有模型

你可以调用`sequelize.sync()`来自动同步所有模型,而不是为每个模型调用`sync()`.



#### 1.8 查询

一些简单的查询如下所示:

```javascript
// 查找所有用户
User.findAll().then(users => {
  console.log("All users:", JSON.stringify(users, null, 4));
});

// 创建新用户
User.create({ firstName: "Jane", lastName: "Doe" }).then(jane => {
  console.log("Jane's auto-generated ID:", jane.id);
});

// 删除所有名为“Jane”的人
User.destroy({
  where: {
    firstName: "Jane"
  }
}).then(() => {
  console.log("Done");
});

// 将所有没有姓氏的人改为“Doe”
User.update({ lastName: "Doe" }, {
  where: {
    lastName: null
  }
}).then(() => {
  console.log("Done");
});
```



#### 2. 方言

Sequelize 独立于特定方言. 这意味着你必须自己将相应的连接器库安装到项目中.

##### 2.1 MySQL

>  $ npm install --save mysql2

为了让 Sequelize 与 MySQL 一起更好地工作,你需要安装 `mysql2@^1.5.2	` 或更高版本. 一旦完成,你可以像这样使用它:

```javascript
const sequelize = new Sequelize('database', 'username', 'password', {
  dialect: 'mysql'
})


// 方法2: 传递连接 URI
const sequelize = new Sequelize('mysql://user:pass@example.com:5432/dbname');
```

**注意:** 你可以通过设置 `dialectOptions` 参数将选项直接传递给方言库.



#### 3. Datatypes - 数据类型

https://demopark.github.io/sequelize-docs-Zh-CN/data-types.html



#### 4. Model definition - 模型定义

- 要定义模型和表之间的映射,请使用 `define` 方法. 每列必须具有数据类型

  ```javascript
  class Project extends Model {}
  Project.init({
    title: Sequelize.STRING,
    description: Sequelize.TEXT
  }, { sequelize, modelName: 'project' });
  
  class Task extends Model {}
  Task.init({
    title: Sequelize.STRING,
    description: Sequelize.TEXT,
    deadline: Sequelize.DATE
  }, { sequelize, modelName: 'task' })
  ```

##### 时间戳

默认情况下,Sequelize 会将 `createdAt` 和 `updatedAt` 属性添加到模型中,以便你能够知道数据库条目何时进入数据库以及何时被更新.





#### 5. Hooks - 钩子

Hook(也称为生命周期事件)是执行 sequelize 调用之前和之后调用的函数. 例如,如果要在保存模型之前始终设置值,可以添加一个 `beforeUpdate` hook.



## 声明 Hook

Hook 的参数通过引用传递. 这意味着你可以更改值,这将反映在insert / update语句中. Hook 可能包含异步动作 - 在这种情况下,Hook 函数应该返回一个 promise.

目前有三种以编程方式添加 hook 的方法:

```javascript
// 方法1 通过 .init() 方法
class User extends Model {}
User.init({
  username: DataTypes.STRING,
  mood: {
    type: DataTypes.ENUM,
    values: ['happy', 'sad', 'neutral']
  }
}, {
  hooks: {
    beforeValidate: (user, options) => {
      user.mood = 'happy';
    },
    afterValidate: (user, options) => {
      user.username = 'Toni';
    }
  },
  sequelize
});

// 方法2 通过 .addHook() 方法
User.addHook('beforeValidate', (user, options) => {
  user.mood = 'happy';
});

User.addHook('afterValidate', 'someCustomName', (user, options) => {
  return Promise.reject(new Error("I'm afraid I can't let you do that!"));
});

// 方法3 通过直接方法
User.beforeCreate((user, options) => {
  return hashPassword(user.password).then(hashedPw => {
    user.password = hashedPw;
  });
});

User.afterValidate('myHookAfter', (user, options) => {
  user.username = 'Toni';
});
```

##### 全局 / 通用 Hook

全局 hook 是所有模型的 hook. 他们可以定义你想要的所有模型的行为,并且对插件特别有用. 它们可以用两种方式来定义,它们的语义略有不同:
###### 默认 Hook (Sequelize.options.define)

```javascript
const sequelize = new Sequelize(..., {
    define: {
        hooks: {
            beforeCreate: () => {
                // 做些什么
            }
        }
    }
});
```

这将为所有模型添加一个默认 hook,如果模型没有定义自己的 `beforeCreate` hook,那么它将运行.

```javascript
class User extends Model {}
User.init({}, { sequelize });
class Project extends Model {}
Project.init({}, {
    hooks: {
        beforeCreate: () => {
            // 做些其它什么
        }
    },
    sequelize
});

User.create() // 运行全局 hook
Project.create() // 运行其自身的 hook (因为全局 hook 被覆盖)
```

## 关联

在大多数情况下,hook 对于相关联的实例而言将是一样的,除了几件事情之外.

1. 当使用 add/set 函数时,将运行 beforeUpdate/afterUpdate hook.

2. 调用 beforeDestroy/afterDestroy hook 的唯一方法是与 `onDelete:'cascade` 和参数 `hooks:true` 相关联. 例如:

```javascript
   class Projects extends Model {}
   Projects.init({
     title: DataTypes.STRING
   }, { sequelize });
   
   class Tasks extends Model {}
   Tasks.init({
     title: DataTypes.STRING
   }, { sequelize });
   
   Projects.hasMany(Tasks, { onDelete: 'cascade', hooks: true });
   Tasks.belongsTo(Projects);
```





#### 8. [Querying - 查询]( https://demopark.github.io/sequelize-docs-Zh-CN/querying.html)

 

