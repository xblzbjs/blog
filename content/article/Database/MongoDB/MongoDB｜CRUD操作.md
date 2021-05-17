---
title: "MongoDB｜CRUD操作"
date: 2021-03-17T11:26:44+08:00
draft: false
author: "[xblzbjs]"
categories: ["数据库"]
tags: ["MongoDB"]
---

# MongoDB的CRUD操作



## Create(创建)

### insertOne()方法

```shell
### insertOne()方法

# 参数含义
db.collection·insertOne() 
db.<collection>·insertOne(
	<document> writeConcern：<document>	
	# <collection>表示写入的集合
	# <document>要替换成要写入的文档本身
	# writeConcern：<document>定义了本次文档创建操作的安全写级别
) 
# 简单来说，安全写级别用来判断一次数据库写入操作是否成功 安全写级别越高，丢失数据的风险就越低，然而写入操作的延迟也可能更高。如果不提供writeconcern文档，用默认的安全写级别。

# 创建单个文档
>  db.accounts.insertOne(	# 虽然没有创建过accounts的集合，但该命令会自动创建相应的集合
...  {
...    _id: "account1",
...    name: "alice",
...    balance: 100,
...  }
... )
{ "acknowledged" : true, "insertedId" : "account1" }
# "acknowledged": true表示安全写级别被启用
# 由于在db.collection.insertone()命令中没有提供writeConcern文档，这里显示的是默认的安全写级别启用状态
# "insertedId"显示了被写入的文档的_id


# 重复_id创建一个新文档及相对应的报错信息
> db.accounts.insertOne(
...   {
...     _id: "account1",
...     name: "bob",
...     balance: 50
...   }
... )
2020-12-25T12:07:21.913+0000 E QUERY    [js] WriteError: E11000 duplicate key error collection: test.accounts index: _id_ dup key: { : "account1" } :
WriteError({
        "index" : 0,
        "code" : 11000,
        "errmsg" : "E11000 duplicate key error collection: test.accounts index: _id_ dup key: { : \"account1\" }",
        "op" : {
                "_id" : "account1",
                "name" : "bob",
                "balance" : 50
        }
})
WriteError@src/mongo/shell/bulk_api.js:461:48
Bulk/mergeBatchResults@src/mongo/shell/bulk_api.js:841:49
Bulk/executeBatch@src/mongo/shell/bulk_api.js:906:13
Bulk/this.execute@src/mongo/shell/bulk_api.js:1150:21
DBCollection.prototype.insertOne@src/mongo/shell/crud_api.js:252:9
@(shell):1:1

# 处理抛出的错误
> try { 
		db.accounts.insertOne( 
			{ 
				_id:"account1", 
				nem:"bob", 
				balance:50 
			} 
		)
	  } catch(e) { 
	      print(e) 
	  }

WriteError({
        "index" : 0,
        "code" : 11000,
        "errmsg" : "E11000 duplicate key error collection: test.accounts index: _id_ dup key: { : \"account1\" }",
        "op" : {
                "_id" : "account1",
                "nem" : "bob",
                "balance" : 50
        }
})

# 省略输入的_id字段，自动生成_id
> db.accounts.insertOne(
... 	{
... 		name: "bob",
...			balance: 50
... 	}
... )
{
        "acknowledged" : true,
        "insertedId" : ObjectId("5fe5d97f6cf5f5ae20ce0a5a") # 自动生成ObjectId
}
```

### insertMany()方法

```shell
### insertMany()方法
db.<collection>.insertMany(
    [<document1>,<document2>,...],
    {
        writeConcern: <document>,
        # ordered决定是否按顺序来写入这些文档。若设置为false，则打乱顺序并优化写入操作的性能
        ordered: <boolean>	
    }
)

# 创建多个文档
> db.accounts.insertMany( [
...     {
...         name: "charlie",
...         balance: 500
...     },
...     {
...         name: "david",
...         balance: 200
...     }
... ] )
{
        "acknowledged" : true,
        "insertedIds" : [
                ObjectId("5fe5e2656cf5f5ae20ce0a5b"),
                ObjectId("5fe5e2656cf5f5ae20ce0a5c")
        ]
}


# 顺序写入时遇到错误
> try {
...   db.accounts.insertMany( [
...     {
...       _id: "account1",
...       name: "edwrad",
...       balance: 700
...     },
...     {
...       name: "fred",
...       balance: 20
...     }
... ] )
... } catch(e) {
...   print(e)
... }
BulkWriteError({
        "writeErrors" : [
                {
                        "index" : 0,
                        "code" : 11000,
                        "errmsg" : "E11000 duplicate key error collection: test.accounts index: _id_ dup key: { : \"account1\" }",
                        "op" : {
                                "_id" : "account1",
                                "name" : "edwrad",
                                "balance" : 700
                        }
                }
        ],
        "writeConcernErrors" : [ ],
        "nInserted" : 0,	# 由于顺序写入，一旦遇到错误文档，剩余的文档无论正确错误都不会写入
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
})

# 乱序写入时遇到错误
> try {
...   db.accounts.insertMany( [
...     {
...       _id: "account1",
...       name: "edwrad",
...       balance: 700
...     },
...     {
...       name: "fred",
...       balance: 20
...     }
... ], { ordered: false } )
... } catch(e) {
...   print(e)
... }
BulkWriteError({
        "writeErrors" : [
                {
                        "index" : 0,
                        "code" : 11000,
                        "errmsg" : "E11000 duplicate key error collection: test.accounts index: _id_ dup key: { : \"account1\" }",
                        "op" : {
                                "_id" : "account1",
                                "name" : "edwrad",
                                "balance" : 700
                        }
                }
        ],
        "writeConcernErrors" : [ ],
        "nInserted" : 1,	# 插入的文档个数为1
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
})
```

### insert()方法

```shell
### insert()方法
db.<collection>.insert(
	<document or array of documents>,
	{
		writeConcern: <document>,
		ordered: <boolean>
	}
)

# 创建单个或多个文档
> db.accounts.insert(
... {
...   name: "george",
...   balance: 1000
... }
... )
WriteResult({ "nInserted" : 1 })
```

### insertOne, insertMany和insert的区别

1. 三个命令返回的结果文档格式不一样

```shell
# insertOne返回的正确结果文档
{ "acknowledged" : true, "insertedId" : "account1" }

# insertOne返回的错误结果文档
WriteError({
        "index" : 0,
        "code" : 11000,
        "errmsg" : "E11000 duplicate key error collection: test.accounts index: _id_ dup key: { : \"account1\" }",
        "op" : {
                "_id" : "account1",
                "name" : "bob",
                "balance" : 50
        }
})
```

```shell
# insertMany返回的正确结果文档
{ "acknowledged" : true, "insertedId" : "account1" }

# insertMany返回的错误结果文档
WriteError({
        "index" : 0,
        "code" : 11000,
        "errmsg" : "E11000 duplicate key error collection: test.accounts index: _id_ dup key: { : \"account1\" }",
        "op" : {
                "_id" : "account1",
                "name" : "bob",
                "balance" : 50
        }
})
```

```shell
# insert写入单个文档返回的正确结果文档
WriteResult({ "nInserted" : 1 })

# insert写入单个文档返回的错误结果文档
WriteResult({
        "nInserted" : 0,
        "writeError" : {
                "code" : 11000,
                "errmsg" : "E11000 duplicate key error collection: test.accounts index: _id_ dup key: { : \"account1\" }"
        }
})

# insert写入多个文档返回的正确结果文档
BulkWriteResult({
        "writeErrors" : [ ],
        "writeConcernErrors" : [ ],
        "nInserted" : 2,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
})

# insert写入多个文档返回的错误结果文档
BulkWriteResult({
        "writeErrors" : [
                {
                        "index" : 0,
                        "code" : 11000,
                        "errmsg" : "E11000 duplicate key error collection: test.accounts index: _id_ dup key: { : \"account1\" }",
                        "op" : {
                                "_id" : "account1",
                                "name" : "jimmy",
                                "balance" : 333
                        }
                }
        ],
        "writeConcernErrors" : [ ],
        "nInserted" : 0,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]  
```

2. insertOne和insertMany命令不支持db.collection.explain()命令；insert支持db.collection.explain()命令

### save()命令

```shell
# save()方法
db.collection.save(
	<document>,
	{
		writeConcern: <document>
	}
)

# 当db.collection.save()命令处理一个新文档时，它会调用db.collection.insert()命令(返回文档与insert方法相同)
```

### 对象主键和复合主键

```shell
> ObjectId()
ObjectId("5fe5f0416cf5f5ae20ce0a65")
> ObjectId().getTimestamp()	# 提取ObjectId的创建时间
ISODate("2020-12-25T13:59:38Z")
> ObjectId("5fe5f0416cf5f5ae20ce0a80")	# 重新赋值ObjectId
ObjectId("5fe5f0416cf5f5ae20ce0a80")
```

复合主键可以使用文档作为文档主键，但仍然要满足主键的唯一性

```shell
# 第一次创建
> db.accounts.insert(
...   {
...     _id: { accountNo: "101", type: "savings" },
...     name: "irene",
...     balance: 80
...   }
... )
WriteResult({ "nInserted" : 1 })
# 第二次创建
> db.accounts.insert(   {     _id: { accountNo: "101", type: "savings" },     name: "irene",     balance: 80   } )
WriteResult({
        "nInserted" : 0,
        "writeError" : {
                "code" : 11000,
                "errmsg" : "E11000 duplicate key error collection: test.accounts index: _id_ dup key: { : { accountNo: \"101\", type: \"savings\" } }"
        }
})
```



## Read(读取)

 



## Update(更新)





## Delete(删除)





