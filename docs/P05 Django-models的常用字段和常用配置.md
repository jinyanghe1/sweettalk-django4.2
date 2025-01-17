---
title: P05 Django-models的常用字段和常用配置
date: 2023-06-25T12:55:30Z
lastmod: 2023-06-27T17:23:30Z
---

# P05 Django-models的常用字段和常用配置

## 常用字段

　　​`CharField`​：用于存储字符串类型，有最大长度限制

　　​`IntegerField`​：用于存储整数类型

　　​`FloatField`​：用于存储浮点数类型

　　​`BooleanField`​：用于存储布尔类型

　　​`DateField`​：用于存储日期类型

　　​`DateTimeField`​：用于存储日期和时间类型

　　​`ImageField`​：用于存储图片类型

　　​`FileField`​：用于存储文件类型

　　​`ForeignKey`​：**外键** 用于表示数据库表之间的关联关系

　　​`OneToOneField`​：**一对一** 用于表示一对一的关联关系

　　​`ManyToManyField`​：**多对多** 用于表示多对多的关联关系

　　‍

## 常用配置

* ​`max_length`​：字段的最大长度限制，可以应用于多种不同的字段类型。
* ​`verbose_name`​：字段的友好名称，便于在管理员后台可视化操作时使用。
* ​`default`​：指定字段的默认值。
* ​`null`​：指定字段是否可以为空。`null=True`​ 设置允许该字段为 NULL 值
* ​`blank`​：指定在表单中输入时是否可以为空白。
* ​`choices`​：用于指定字段的可选值枚举列表。

  在最上面定义

  ```python
  class DeliveryMaterial(Model):
      """复核产品"""

      class Status(TextChoices):
          """状态"""

          QUALIFIED = ('qualified', '良品')
          UNQUALIFIED = ('unqualified', '不良品')

      status = CharField(max_length=32, choices=Status.choices, default=Status.QUALIFIED, verbose_name='状态')
  ```

  ​`TextChoices`​ 是 Django 3.0 引入的一个枚举类，用于在模型字段中创建可选择的、文本值的选项。
* ​`related_name`​：指定在多对多等关系中反向使用的名称。
* ​`on_delete`​：指定如果外键关联的对象被删除时应该采取什么操作。

　　‍

　　‍

　　‍
