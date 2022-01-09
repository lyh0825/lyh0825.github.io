---
title: Mongodb
date: 2021-08-28 11:38:04
tags: Mongodb
---

# Mongodb

## 推荐阅读

[**mongodb时间戳**](https://www.codenong.com/6452021/)

## _id

打开 MongoDB 数据库中的任何一个文档，你会注意到文档中有一个 _id 字段，实际上，ObjectId 或 _id 是每个 MongoDB 文档中都存在的字段。

### bjectId 的结构

这些是 _id 的一些主要特征的摘要：

- _id 是集合中文档的主键，用于区分文档（记录）。
- _id自动编入索引。指定 { _id: } 的查找将 _id 索引作为其指南。
- 默认情况下，_id 字段的类型为 ObjectID，是 MongoDB 的 BSON 类型之一。如果需要，用户还可以将 _id 覆盖为 ObjectID 以外的其他内容。

ObjectID 长度为 12 字节，由几个 2-4 字节的链组成。每个链代表并指定文档身份的具体内容。以下的值构成了完整的 12 字节组合：

**一个字节两个字符**

- 一个 4 字节的值，表示自 Unix 纪元以来的秒数-->8位16进制
- 一个 3 字节的机器标识符
- 一个 2 字节的进程 ID
- 一个 3 字节的计数器，以随机值开始

![img](http://www.navicat.com.cn/link/Blog/Image/2019/20190326/_id.png)

通常，你不必担心要如何生成 ObjectID。如果文档尚未分配 _id 值，MongoDB 将自动生成一个 _id 值。

### 创建新的 ObjectId

如果你想自己生成新的 ObjectId，可以使用以下代码：

```shell
newObjectId = ObjectId()
```

你也可以直接在 Navicat 编辑器中输入它。

这将会生成一个唯一的 _id，例如：

```shell
ObjectId("5349b4ddd2781d08c09890f3")
```

或者，你亦可以提供一个 12 字节的 ID：

```shell
myObjectId = ObjectId("5349b4ddd2781d08c09890f4")
```

### 文档的创建时间戳

由于 _id ObjectId 默认存储了 4 字节的时间戳，因此在大多数情况下，你不需要存储任何文档的创建时间。你可以使用 getTimestamp 方法获取文档的创建时间：

```shell
ObjectId("5349b4ddd2781d08c09890f4").getTimestamp()
>>
ObjectId("612b264173e5b4a6bda0abdb")
```

这将以 ISO 日期格式返回此文档的创建时间

```shell
ISODate("2019-09-12T30:39:17Z")
```

### 将 ObjectId 转换为字符串（String）

在某些情况下，你可能需要得到字符串格式的 ObjectId 值。若要转换 ObjectId 为字符串，请使用以下代码：

```shell
newObjectId.str
```

上面的代码将返回 Guid 的字符串格式的 ObjectId：

```shell
5349b4ddd2781d08c09890f3
```

### 文档排序

由于每个 ObjectId 都包含一个时间戳，因此你可以用 _id 字段将文档按创建时间排序。但请务必注意，此排序方法并不代表排序是严格或精确的，因为 ID 的其他组件也会影响，导致次序会反映其他变量，而不仅仅是创建时间。

### 更改 ObjectId

_id 字段基本上是不可变的。在创建文档之后，根据定义，它已被分配了一个无法更改的 _id。话虽如此，在插入新文档时是可以覆盖 _id 的。覆盖文档的 _id 字段可能有其用处，但是当这样做时，你有责任确保每个文档的 _id 值都是唯一的。
