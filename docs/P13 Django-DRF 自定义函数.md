---
title: P13 Django-DRF 自定义函数
date: 2023-06-30T20:53:57Z
lastmod: 2023-06-30T21:01:25Z
---

# P13 Django-DRF 自定义函数

## 是什么

　　​`from rest_framework.decorators import action`​

　　​`@action` ​是 Django REST framework 中的一个装饰器，用于将自定义函数转换为视图集的一个动作。`@action` ​装饰器提供了一种定义自定义函数的方式，这些函数并不直接对应于标准的 CRUD 操作（Create-Read-Update-Delete），而是实现一些其他的自定义行为或业务逻辑。

## 如何使用

**views.py**

```python
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

　　其中，`detail=False` ​表示该动作不需要处理单个对象，而是处理整个集合；

　　被 `@action` ​装饰的函数需要作为方法定义在视图集类中，并且在使用 `router.register()` ​注册视图集时，需要指定 `basename` ​参数，以确保该动作的 URL 能够正确映射到视图集。

　　‍
