[TOC]

# 1. MongoDB

## 1.0 MongoDB原生的 CRUD 命令

### 1.1.1 C creat 创建数据库数据

```js
db.集合名.insert(文档对象);
db.集合名.insertOne(文档对象);
db.集合名.insertMany([文档对象,文档对象]);

// 插入一个: 即将废弃
db.students.insert({name:"kobe",age:18,sex:"male"});
// 插入一个
db.students.insertOne({name:"Gene",age:25,sex:"male"});
// 插入多个
db.students.insertMany([
	{name:"贾娇娇",age:24,sex:"female"},
	{name:"任甜甜",age:23,sex:"female"},
	{name:"杨阳阳",age:24,sex:"male"},
	{name:"张三",age:18,sex:"male"},
	{name:"李四",age:22,sex:"male"},
]);
```

### 1.1.2 R read  读取数据

```js
db.集合名.find(查询条件[,投影])
    举例:db.students.find({age:18}),查找年龄为18的所有信息
    举例:db.students.find({age:18,name:'jack'}),查找年龄为18且名字为jack的学生
    
常用操作符：
    1. < , <= , > , >= , !==   对应为： $lt $lte $gt $gte $ne
        举例：db.集合名.find({age:{$gte:20}}),年龄是大于等于20的
    2.逻辑或：$in 或 $or
        查找年龄为18或20的学生
        举例：db.students.find({age:{$in:[18,20]}})
        举例：db.students.find({$or:[{age:18},{age:20}]})
        3.逻辑非：$nin
    4.正则匹配：
        举例：db.students.find({name:/^T/})
    5.$where能写函数：
        db.students.find({$where:function(){
            return this.name === 'zhangsan' && this.age === 18
        }})
            
投影：过滤掉不想要的数据，只保留想要展示的数据
    举例：db.students.find({},{_id:0,name:0}),过滤掉id和name
    举例：db.students.find({},{age:1}),只保留age
    
补充：db.集合名.findOne(查询条件[,投影])，默认只要找到的第一个
```

> **code** 

```js
#1. 条件查询
	db.students.find({age:18});
	db.students.find({age:18,name:"张三"});

#2. 条件查询
  db.students.find({age:{$gte:24}});
  // 查询年龄为 24 或 25 的学生
  db.students.find({age:{$in:[24,25]}});
```

### 1.1.3 U update 更新数据库

```js
db.集合名.update(查询条件,要更新的内容[,配置对象])
    
//如下会将更新内容替换掉整个文档对象，并且_id不受影响
    举例：db.students.update({name:'zhangsan'},{age:19})
    
//使用$set修改指定内容，其他数据不变，不过只能匹配一个zhangsan
    举例：db.students.update({name:'zhangsan'},{$set:{age:19}})
    
//修改多个文档对象，匹配多个zhangsan,把所有zhangsna的年龄都替换为19
    举例：db.students.update({name:'zhangsan'},{$set:{age:19}},{multi:true})
    
 补充：db.集合名.updateOne(查询条件,要更新的内容[,配置对象])
       db.集合名.updateMany(查询条件,要更新的内容[,配置对象])
```

### 1.1.4 D delete 删除数据库数据

```js
db.集合名.remove(查询条件)
    //删除所有年龄小于等于19的学生
    举例：db.students.remove({age:{$lte:19}}) 
```





