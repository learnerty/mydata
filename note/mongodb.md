## mongodb
#### 安装
1. `sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`
2. `echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`
3. `sudo apt-get update`
4. `sudo apt-get install -y mongodb-org`

#### 查看版本
`mongod --version`

### 创建data文件夹
`mkdir data`
#### 启动服务端
`mongod --dbpath=./data`

#### 启动客户端连接服务器
`mongo --host=127.0.0.1` 或者 `mongo` 按回车键   --host后的值表示服务器的ip地址

#### 查看有哪些数据库
`show dbs`

#### 新建或切换数据库
`use test`  test是数据库的名称

#### 查看当前使用的数据库
`db` 或 `db.getName()`

#### 删除数据库
`db.dropDatabase()` 切换到要删的数据库执行

#### 查看有那些集合(数据库表)
`show tables` 或  `show collections`

#### 插入集合
`db.users.insert({name:'tian',age:23})`  users是集合的名称

#### 查询集合内容
`db.users.find({name:'tian'})` 查询users集合内的内容，find内传入查询条件，不传默认查找全部

#### 删除集合
`db.users.remove({name:'tian'})` 传入一个{}空条件删除所有

#### 更新数据
`db.users.update({name:'tian'},{$set:{age:22}})` 更新name为tian的age为22,也可以增加不存在的属性

#### save()属性
不传id，插入新文档，传入id，把id符合的覆盖掉
`db.users.save({_id:ObjectId("592e78c76266fab949e54500"),name:'hahaha'})`

#### 返回集合所有数量
`db.users.find().count()`

#### 跳过 skip
`db.users.find().skip(2)`  跳过两条  
`db.users.find().skip(1).limit(1)`  跳过一条查一条

#### 删除集合
`db.users.drop()`
