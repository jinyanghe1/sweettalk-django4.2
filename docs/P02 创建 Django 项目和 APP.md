---
title: P02 创建 Django 项目和 APP
date: 2023-06-20T10:44:27Z
lastmod: 2023-06-27T17:23:22Z
---

# P02 创建 Django 项目和 APP

### 步骤

1. 安装 django

    ​`pip install django`​​​​
2. 创建项目

    ​`django-admin startproject projectname`​​​​

    其中 `projectname`​​​​ 指的是你的项目名字
3. 创建 app

    ​`python manage.py startapp appname`​​​​

    其中 `appname`​​​​ 指的是你的应用名字

    创建完成后，需要到 `settings.py`​​ ​中注册

    **在指定路径下创建 app：**

    * 先 cd 到指定路径
    * 运行 `django-admin startapp appname`​​
    * 打开 app 下 apps.py 文件
    * 将 name 变量赋值修改

      ​![image](assets/image-20230531125638-pq3meyv.png)​

　　‍
