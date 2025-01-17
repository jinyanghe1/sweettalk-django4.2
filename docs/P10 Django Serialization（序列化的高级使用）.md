---
title: P10 Django Serialization（序列化的高级使用）
date: 2023-06-27T17:22:54Z
lastmod: 2023-06-28T16:33:15Z
---

# P10 Django Serialization（序列化的高级使用）

　　序列化器 `serializers`​

　　**序列化器的作用：**

1. 序列化：将 `queryset`​ 和 `instance`​ 转换为 `json/xml/yaml`​ 返回给前端
2. 反序列化：与序列化则相反

　　**定义序列化器：**

1. 定义类，继承自 Serializer

　　通常新建一个 `serializers.py`​ 文件 撰写序列化内容

　　suah as：

　　目前只支持

　　read_only：只读

　　label：字段说明信息

　　max_length：最大长度

**serializer.py**

```python
# 定义产品序列化器
from rest_framework.serializers import *
from .models import *

# 产品分类序列化器
class GoodsCategorySerializer(ModelSerializer):
    class Meta:
        model = GoodsCategory
        fields = ('name', 'remark')

# 产品序列化器
class GoodsSerializer(ModelSerializer):
    # 外键字段相关的数据 需要单独序列化
    category = GoodsCategorySerializer()

    class Meta:
        model = Goods

        # 序列化单个字段
        fields = ('name',)

        # 序列化多个字段
        fields = ('name','number',)



        # 序列化所有字段
        fields = '__all__'

```

**views.py**

```python
from django.shortcuts import render
from rest_framework.response import Response
from .models import *
from rest_framework.decorators import api_view
from django.shortcuts import get_object_or_404
from rest_framework.views import APIView
from .serializer import *

class GetGoods(APIView):
    def get(self,request):
        queryset = Goods.objects.all()
        serializer = GoodsSerializer(instance=queryset,many = True)
        print(serializer.data)
        return Response(serializer.data)
```

**urls.py**

```python
from django.contrib import admin
from django.urls import path
from apps.erp_test.views import *

urlpatterns = [
    path('admin/', admin.site.urls),
    path('filtergoodscategory/', FilterGoodsCategory),
    path('filtergoodscategoryapi/', FilterGoodsCategoryAPI.as_view()),
    path('getgoods/', GetGoods.as_view()),
]
```

* 序列化单个对象

  * 获取对象  `data = Goods.objects.get(id=1)`​
  * 创建序列化器 `sberializer = GoodsSerializer(instance=data)`​
  * 转换数据 `print(serializer.data)`​
  * 注意点：

    ​`instance`​是一个参数，用于指定要序列化或反序列化的 Python 对象。具体来说，它是一个类实例(Class Instance),通常是指一个从数据库或其他数据源中检索出来的模型实例(Model Instance)。

    当我们需要将一个模型实例转换为 JSON 或其他格式时，可以使用 Django 的序列化器(Serializer)来实现。
  * 输出：

    ```json
    {'id': 1, 'number': '1', 'name': '第一个产品', 'purchase_price': 100.0, 'retail_price': 150.0, 'remark': '测试产品'}
    ```

* 序列化多个对象

  ```python
  data = Goods.objects.all() # 获取对象

  # 创建序列化器，many表示序列化多个对象，默认为单个
  serializer = GoodsSerializer(instance=data,many=True)

  print(serializer.data) # 转换数据

  # 输出：
  [OrderedDict([('id', 1), ('number', '1'), ('name', '第一个产品'), ('purchase_price', 100.0), ('retail_price', 150.0), ('remark', '测试产品')]), OrderedDict([('id', 2), ('number', '123'), ('name', '产品2'), ('purchase_price', 123.0), ('retail_price', 4123.0), ('remark', '测试产品2')])]  
  ```
