---
title:P11 Django-DRF（ModelViewSet）
date: 2023-06-28T17:10:35Z
lastmod: 2023-06-29T23:47:37Z
---

# P11 Django-DRF（ModelViewSet）

## 是什么？

　　ModelViewSet 是 Django REST framework 提供的一个视图集类，它封装了常见的模型操作方法。

　　模型类提供了默认的增删改查功能。

　　它继承自 `GenericViewSet`​、`ListModelMixin`​、`RetrieveModelMixin`​、`CreateModelMixin`​、`UpdateModelMixin`​、`DestoryModelMixin`​。

|知识点|请求|url|特点|
| ------------------| --------| --------------------------| ----------------------------------------|
|GenericViewSet|||提供一组通用的视图方法，方便实现特定功能|
|ListModelMixin|get|127.0.0.1:8000/book/|提供 `list`​ 方法，用于**获取资源列表**|
|RetrieveModelMixin|get|127.0.0.1:8000/book/{1}/|提供 `retrieve`​ 方法，用于**获取单个资源的详细信息**|
|CreateModelMixin|post|127.0.0.1:8000/book/|提供 `create`​ 方法，用于**创建资源**|
|UpdateModelMixin|put|127.0.0.1:8000/book/{1}/|提供 `update`​ 方法，用于**更新资源**|
|DestroyModelMixin|detete|127.0.0.1:8000/book/{1}/|提供 `destroy`​ 方法，用于**删除资源**|
|自定义|get/post|127.0.0.1:8000/book/自定义|用户自定义方法/函数|

　　这些技术知识点可以配合使用，帮助我们快速构建出具有 CRUD 功能的 Web 应用，并且遵循了 Django 框架的惯例和最佳实践。它们的应用场景包括博客系统、电商平台、社交网络等各种类型的 Web 应用。通过使用这些技术知识点，我们能够提高开发效率，减少重复的代码编写工作，并且保证代码的一致性和可维护性。

## 如何使用

　　设置 `queryset`​ 属性为要查询的对象集合，并设置 `serializer_class`​ 属性为对应的序列化器类。

### 示例

**view.py**

```python
from rest_framework.viewsets import ModelViewSet
class YourModelViewSet(ModelViewSet):
    queryset = YourModel.objects.all()
    serializer_class = YourModelSerializer
```

　　使用 ModelViewSet 后，你将自动获得默认的 CRUD 方法。


```python
from rest_framework.decorators import action
#### modelviewset
class GoodsCategoryViewSet(ModelViewSet):
    # 指定查询集（用到的数据）
    queryset = GoodsCategory.objects.all()
    # 指定查询集用到的序列化容器
    serializer_class = GoodsCategorySerilizer

    @action(detail=False, methods=['get'])
    def latest(self, request):
        latest_obj = GoodsCategory.objects.latest('id')
        print(latest_obj)
        return Response("helllo 你调用了自定义的函数")
```

**serializer.py**

```python
class GoodsSerializer(ModelSerializer):

    # 外键字段相关的数据 需要单独写
    category = GoodsCategorySerilizer()

    class Meta:
        # 指定需要序列化的表
        model = Goods
        # 指定我们需要序列化的字段
        fields = '__all__'
```
