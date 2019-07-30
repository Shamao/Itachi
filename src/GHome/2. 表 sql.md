####  创建用户

```sql
Create TABLE user
(
  id       integer(10) not null auto_increment primary key,
  username varchar(10) comment '用户名'
) engine=innoDB DEFAULT  charset = utf8  comment='用户信息表';



```



####  物品表
```
Create TABLE goods
(
  id           integer(10) not null auto_increment primary key,
  name         varchar(10) comment '名称',
  count        integer comment '数量',
  unit         varchar(10) comment '单位',
  start_date   date comment '生产日期',
  fresh_period int comment '保质期',
  end_date     date comment '结束日期',
  source       integer comment '购买来源',
  img_url      Text comment '图片资源',
  user_id      integer(10),
  FOREIGN KEY (user_id) REFERENCES user (id)
) engine = innoDB
  DEFAULT charset = utf8 comment ='物品表';
