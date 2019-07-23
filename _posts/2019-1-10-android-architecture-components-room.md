---
layout: post
title: Android Architecture Components Room
date: 2019-1-10 20:00:00 +0800
categories: Android
tag: [Architecture Components, Room]
---

* content
{:toc}

>本文所有内容扩展自阅读：[https://developer.android.com/training/data-storage/room/](https://developer.android.com/training/data-storage/room/)

>不错的相关文章： [https://medium.com/androiddevelopers/7-pro-tips-for-room-fbadea4bfbd1](https://medium.com/androiddevelopers/7-pro-tips-for-room-fbadea4bfbd1)


@AutoValue
====================================
>[https://www.baeldung.com/introduction-to-autovalue](https://www.baeldung.com/introduction-to-autovalue)

这个是一个用于Java的自动生成模板代码的注解。生成的模板代码中的变量都是final的，且不提供`setter`函数。会重写`equals`和`hashCode`。在对象地址或里面的属性值都相同的情况下返回`true`。


@PrimaryKey @ColumnInfo等
======================================
这几个参数以下面为例子  
`@PrimaryKey`： 设置表的主`key`  
`@PrimaryKeys`: 可以将两个列拼成一个key，每一行拼好的这个key必须是不一样的。
`@ColumnInfo`： 更改变量所在列名，如果没有这个就是默认变量名  
`@Index`： 给表添加索引，用于快速`query`  
`unique`： 说明前面两个的两个列`first_name`和`last_name`不能重复  
`@Ignore`: 持久化的时候忽略  
`@Embedded`： 将另一个表嵌入当前表中  
`@ForeignKey`： 引用另一个表，`Room`[不支持显示引用](https://developer.android.com/training/data-storage/room/referencing-data.html#understand-no-object-references)
```bash
@Entity(indices = arrayOf(Index(value = ["first_name", "last_name"],
        unique = true)))
data class User(
    @PrimaryKey var id: Int,
    @ColumnInfo(name = "first_name") var firstName: String?,
    @ColumnInfo(name = "last_name") var lastName: String?,
    @Ignore var picture: Bitmap?
)
```
-----------------------------------------------------------------------------

@DatabaseView
======================================
这个可以从一张已有的表中，根据一些指定的规则（`SQL SELECT语句`）,形成一个视观表。
例如像以下这种方式：
```bash
CREATE VIEW V_Customer
AS SELECT First_Name, Last_Name, Country
FROM Customer;
```
从表`Customer`中，抽出`First_name`、`Last_name`和`Country`这几列形成。
>来源： [https://www.1keydata.com/tw/sql/sql-create-view.html](https://www.1keydata.com/tw/sql/sql-create-view.html)


@Dao
=====================================
获得app的`Room`数据，`@Dao`解释的可以是`interface`，也可以是`abstract class`，会自动生成操作数据库的方法。有以下快捷操作方法：
* **@Insert**  
  用于直接插入数据，`onConlict`意思为`primary key`冲突的时候的解决方法
  ```bash
  @Insert(onConflict = OnConflictStrategy.REPLACE)
  ```
* **@Update**  
  用于更新数据
* **@Delete**  
  用于删除数据
* **@Query**  
  可用于读/写数据库
  ```bash
  @Query("SELECT * FROM user")
  ```

