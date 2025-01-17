---
title: P08 DRF QuerySet和Instance
date: 2023-06-25T15:37:01Z
lastmod: 2023-06-27T18:16:10Z
---

# P08 DRF QuerySet和Instance

## Teaching Order

1. 介绍数据类型（是什么）

   1. QuerySet
   2. Instance
2. 在哪里

   通过输出观察数据类型

**views.py**

   ```python
   # 函数式编程
   @api_view(['POST','GET'])
   def FilterGoodsCategory(request):
       data = request.data['分类名字']
       goods_category = get_object_or_404(GoodsCategory,name=data)
       print(goods_category)
       ans = Goods.objects.filter(category=goods_category).all().values()
       print(ans)
       print("object type:",type(Goods.objects))
       print("object.all() type:",type(Goods.objects.all()))
       print("object.all().values type:",type(Goods.objects.all().values()))
       print("object.get(id=1) type:",type(Goods.objects.get(id=1)))
       return Response(ans)
   ```

> 介绍 QuerySet 和 Instance 的区别，使用场景，如何使用。

## **QuerySet**

> QuerySet 是 Django 中的一个查询集合，它是由 Model.objects 方法返回的，并且可以用于生成数据库中所有满足一定条件的对象的列表。
>
> 列表中包含多个 Instance

* filter()

  用于返回符合条件的所有数据
* get()

  ​`get()`​​ 方法与 `filter()`​​ 的作用类似，用于返回符合条件的单个对象

  但是可能会返回多个值
* all()

  ​`all()`​​ 方法返回模型的所有对象，它的效果等价于不带任何条件的 `filter()`​​ 方法。
* delete()

  ​`delete()`​​ 方法可以删除符合条件的所有对象
* update()

  ​`update()`​​ 方法可以将符合条件的所有对象的某个字段值进行更新
* create()

  ​`create()`​​ 方法是 `save()`​​ 方法的快捷方式，用于创建并保存一个新的对象。
* count()

  ​`count()`​​ 方法返回符合条件的对象数量
* order_by()

  ​`order_by()`​​ 方法可以对返回的对象进行排序，默认为升序。降序则在字段名前面加负号。
* ​`Map.objects.all().values().first()`​​

## Instance

> Instance 指的是一个 Django 模型的单个实例，也就是数据库中的一行数据。相比于 QuerySet（查询集合），它是针对单个对象的操作，比如创建、更新或者删除模型实例。stance 用于创建、更新或者删除单个模型实例。

* 创建一个对象：`Obj = Model(attr1=val1, attr2=val2)`​，`Obj.save()`​
* 更新一个对象：`Obj = Model.objects.get(id=xxx)`​，`Obj.attr1 = val1`​，`Obj.save()`​
* 删除一个对象：`Obj = Model.objects.get(id=xxx)`​​，`Obj.delete()`​​

## Difference

　　QuerySet 适用于需要查找多个对象或进行聚合操作的场景，而 Instance 适用于单独对象的创建、修改和删除操作。

