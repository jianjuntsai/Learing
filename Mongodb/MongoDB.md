启动mongodb

```
sudo mongod --dbpath=/users/ice399/data/db
```


# imooc:python 操作MongoDB数据库

[MongoDB官方手册](https://docs.mongodb.com/manual/)

##6-1 MongoDB数据库基础

库--集合--文档

MongoDB的要点

- 文档
- 区分大小写
- Key是唯一的，不可重复
- 键值对是有序的

集合

- 就是一组文档，就是关系库里的表
- 文档类似于关系库里的行
- 集合中的文档中无需固定的结构

集合的命名

- 不能为空字符串
- 不符包含\0
- 不能使用system.前缀
- 不用$
- 用.来分割

数据库

- 多个文档组成集合，多个集合组成数据库
- 一个实例可以承载多个数据库
- 每个数据库都有独立的权限
- 保留数据库名称【admin,local,config】

## 6-3命令行操作数据库（CRUD）

CRUD(creat, research, update, delet)

- 查看数据库 ```show dbs```
- 新建/切换数据库 ``` use [name of the database]```
- show collections: 显示数据库中的集合（关系型中叫表，我们要逐渐熟悉）。
- 查看目前所在数据库 ```db```
- 新建collection ``` db.[Collection].insert()```
- 查看文档 

查询所有文档
```db.[Collection].find()```

查询所有男生的姓名和年龄

```
db.info_student.find({sex:'male'},{name:1, age:true, _id:0})
```

查询所有分数大于/小于60分的文档

```
db.info_student.find({grade:{'$gte':60}})
db.info_student.find({grade:{'$lte':60}})
```

查询所有18岁的男生和16岁女生

```
 db.info_student.find({'$or':[{sex:'male',age:18},{sex:'female',age:16}]})
```

按年龄排序（升序/降序）

```
db.info_student.find().sort({age:1})
db.info_student.find().sort({age:-1})
```

- 查看第1个文档 ```db.[Collection].findOne()```
- 修改数据
```
db.[Collection].update({条件},{更新后的文档})
```

所有女生的年龄增加1岁

```
db.info_student.update({sex:'female'},{'$inc':{age:1}},{multi:true})
```



- 删除文档 ```db.[Collection].remove({条件})```

## 6-4 练习

## 6-5 图形工具使用
[Robo 3T](https://robomongo.org/)

## 7 Python 操作MongoDB

### 连接数据库

- 简写 client = MongoClient()
- client = MongoClient('localhost',27017)
- 使用URI client = MongoClient('mongodb://localhost:27017/')

见[ipynb文件 python_mongoDB](http://localhost:8888/notebooks/Desktop/DataScience/MongoDB/python_mongoDB.ipynb)







