# Django_blog 开发记录
## 创建项目
```cmd
# 进入虚拟环境
source django_cmy/bin/activate

# 创建项目目录
mkdir blog_cc

# 创建django项目
django-admin startproject blog 

# 修改blog/settings.py
修改ALLOW_HOSTS=['*']
修改TIME_ZONE='Asia/Shanghai'

```

## 启动项目
```cmd
# 修改过blog/settins.py后，回到manage.py主目录(新版需要migrate一下)
python3 manage.py migrate

# 启动服务
python3 manage.py runserver 0.0.0.0:8000

```

## 创建article 项目

### urls 相关
```cmd
# 在manage.py 主目录创建articles app
python3 manage.py startapp articles

# 在blog/settings.py 注册articles app
INSTALLED_APPS = [
    # ...
    'articles',
]

# 在主urls.py 中将 articles 的urls include 进来
from django.urls import path, include
urlpatterns = [
    # ...
    path('articles/', include('articles.urls', namespace='articles')),
]

# 在articles 目录下创建urls.py，并配置
touch urls.py

from django.urls import path

urlpatterns = [
    # 暂时为空
]
```

### models 相关
```
# 什么是Model?
Model 是数据存取层，处理与数据相关的所有事务，包括如何存取，如何验证有效性，包含哪些行为以及数据之间的关系

# 编写articles/models.py Article_Post class
1. 作者 author
2. 标题 title
3. 正文 body
4. 发表时间 created_time
5. 更新时间 updated_time
排序方式，按创建时间倒序排序，最新的在top
ordering = ('-created_time',)
返回方式，return self.title

# 数据迁移
python3 manage.py makemigrations 
python3 manage.py migrate

# 在这期间会产生编译文件__pycache__，并不希望将这些文件放在git中维护，编辑.gitignore 
**/__pycache__
*.swp
git restore blog/__pychache__/*
git add .gitignore 
git commit -m "add gitignore"

```

### views 相关
```cmd
# articles/views.py 先填一个最简单的hello there，简单测试我们的网站
from django.http import HttpResponse
def articles_list(request):
    return HttpResponse("hello there")

# 将views中的函数关联到用户请求url上
vim articles/urls.py
from . import views 
urlpatterns = [
    # ...
    path('articles_list', views.articles_list, name='articles_list'),
]

# 创建管理员账号
python3 manage.py createsuperuser

# 将发表文章(Article_post)注册到网站后台中去
vim articles/admin.py
from .models import ArticlePost 

admin.site.register(ArticlePost)

# 可以进入网站后台添加几条test数据

# 改写views.py
1. 将所有数据库中的文章取出来
2. 传递给前端templates
3. load templates，返回context对象
from .models import ArticlePost 

def articles_list(request):
    articles = ArticlePost.objects.all();
    context = {'articles':articles}
    return render(request, 'articles/list.html', context)
    
```
### templates 相关
```
# 编写templates
mkdir articles
vim articles/list.html
{% for article in articles%}
    <p>{{article.title}}</p>
{% endfor%}

# 将templates注册到blog/settings.py
import os
TEMPLATES = [
    {
        # ...
        'DIRS': [os.path.join(BASE_DIR,'templates')]
    }
]

# 准备bootstrap相关，目录结构如下
-static
    --bootstrap 
        ---css 
        ---js
    --jquery
        ---jquery-3.6.0.js
        
# 在Django项目中指定静态文件的位置
blog/settings.py
STATICFILES_DIRS = (os.path.join(BASE_DIR,"static"),)

# 写前端代码 base.html footer.html header.html articles/list.html

# 改写articles/views.py
def article_detail(request, id):
    article = ArticlePost.objects.get(id=id)
    context = {'article': article}
    return render(request, 'articles/detail.html', context)
    
# 写前端代码 templates/articles/detail.html

```

### 优化网页入口

```
# header.html 点击文章回到文章列表
<a class="nav-link" href="{% url 'articles:articles_list' %}">文章</a>

# articles/list.html 点击阅读本文进入detail页面
<a href="{% url 'articles:article_detail' article.id %}" class="btn btn-primary">阅读本文</a>


```

### 引入markdown

```
# 在虚拟环境下安装markdown
pip install markdown 

# 改写articles/views.py 中的article_detail函数 
# rendering markdown to html

     article.body = markdown.markdown(article.body,
             extentions=[
             'markdown.extensions.extra',
             'markdown.extentions.codehilite',
              ])

# 修改templates/articles/detail.html
增加|safe ，避免对markdown中的符号进行转义

#  没有代码高亮不行！！！！
mkdir -p static/md_css
pip install Pygments
cd md_css/
pygmentize -S monokai -f html -a .codehilite > monokai.css

# 在base.html 中引入 monokai.css 
<link rel="stylesheet" href="{% static 'md_css/monokai.css' %}">

```

### 文章删除
```
# 增加articles/views.py article_delete函数
def article_delete(request, id):
    article = ArticlePost.objects.get(id=id)
    article.delete()
    return redirect("articles:articles_list")
    
# 增加路由
vim articles/urls.py
urlpatterns = {
    # ...
    path('article_delete/', views.article_delete, name='article_delete')
}

# 修改templates/articles/details.html 在相应的盒子中加入a标签
<div class="col-12 alert alert-success">作者：{{ article.author }} <a href="{% url "articles_article_delete" article.id %}"> 删除文章</a>

# 增加弹窗防止手抖误删
## 添加layer web组件
vim templates/base.html
在jquery-3.6.0.js后面引入layer.js
<script src="{% static 'layer/layer.js'%}"></script>

## 给删除文章添加onclick confirm_delete函数
<script>
    function confirm_delete(){
            layer.open({
                    title: "确认删除",
                    content: "确认删除这篇文章吗？",
                    yes: function(index, layero){
                            location.href='{% url "articles:article_delete" article.id %}'
                        },
                })
        }
</script>

# 如何避免CSRF跨域攻击？
## 利用Django中间件对csrf_token的支持
vim templates/articles/detail.html
<div>
# ...
<a href="#" onclick="confirm_csrf_avoidance_delete">删除文章</a>
<form
        style="display:none;"
        id="csrf_avoidance_delete"
        action="{% url 'articles:article_csrf_avoidance_delete' article.id %}"
        method="POST"
        >
        {% csrf_token %}
        <button type="submit">send</button>    
</div>
<script>
    function confirm_csrf_avoidance_delete(){
        layer.open({
            title: "文章删除",
            content: "确认删除该文章吗？",
            yes :function(index, layero){
                    $(`form#csrf_avoidance_delete button`).onclick();
                    layer.close();
            },
        })
    }
</script> 
## 修改articles/views.py
def article_csrf_avoidance_delete(request, id):
    if request.method = "POST":
        article = ArticlePost.objects.get(id=id)
        article.delete()
        return redirect("articles:articles_list")
    else:
        return HttpResponse("仅支持POST请求")
## 修改articles/urls.py
urlpatterns = [
    # ...
    path('article_csrf_avoidance_delete/<int:id>/', views.article_csrf_avoidance_delete, name='article_csrf_avoidance_delete')
]
```

### 文章更新
```
# 增加视图article_update函数
vim articles/views.py
def article_update(request, id):
    # take out the article
    article = ArticlePost.objects.get(id=id)

    # jude the method
    if request.method == "POST":
        article_post_form = ArticlePostForm(data=request.POST)
        if article_post_form.isvalid():
            # write the new body
            article.title = request.POST['title']
            article.body = request.POST['body']
            article.save()
            return redirect("articles:articles_list", id=id)
        else:
            return HttpResponse("表单内容有误，请重新填写")
    else:
        article_post_form = ArticlePostForm()
        context = {'article': article, 'article_post_form': article_post_form}
        return render(request, 'articles/update.html', context)
# 添加该函数到urls
vim articles/urls.py
urlpatterns = [
    # ...
    path('article_update/<int:id>/', views.article_update, name='article_update')
]
# 增加templates articles/update.html
{% extends "base.html" %} {% load static %}
{% block title %} 更新文章 {% endblock title %}
{% block content %}
<div class="container">
    <div class="row">
        <div class="col-12">
            <br>
            <form method="post" action=".">
                {% csrf_token %}
                <div class="form-group">
                    <label for="title">文章标题</label>
                    <!-- 在 value 属性中指定文本框的初始值为旧的内容，即 article 对象中的 title 字段 -->
                    <input type="text" class="form-control" id="title" name="title" value="{{ article.title }}">
                </div>
                <div class="form-group">
                    <label for="body">文章正文</label>
                    <!-- 文本域不需要 value 属性，直接在标签体中嵌入数据即可 -->
                    <textarea type="text" class="form-control" id="body" name="body" rows="12">{{ article.body }}</textarea>
                </div>
                <button type="submit" class="btn btn-primary">完成</button>
            </form>
        </div>
    </div>
</div>
{% endblock content %}
# 在detail.html 中添加编辑入口
|<a href="{% url 'articles:article_update' article.id %}"> 编辑文章 </a>
```
## 用户登录与登出
```
# 开启一个新的app
python3 manage.py startapp userprofile
cd userprofile
touch urls.py

# 编写登录表单
vim userprofile/forms.py
from django import forms
from django.contrib.auth.models import User

class UserLoginForm(forms.Form):
    username=forms.CharField()
    password=forms.CharField()
    
# 编写视图函数
vim userprofile/views.py
from django.shortcuts import render, redirect
from django.contrib.auth import authenticate, login 
from django.http import HttpResponse
from .forms import UserLoginForm

def user_login(request):
    if request.method == "POST":
        user_login_form = UserLoginForm(data=request.POST)
        if user_login_form.is_valid():
            data = user_login_form.cleaned_data
            user = authenticate(username=data['username'], password=data['passsword'])
            if user:
                login(request, user)
                return redirect("articles:articles_list")
            else:
                return HttpResponse("账号或密码输入错误")
        else:
            return HttpResponse("账号或密码不合法")
    elif request.method == "GET":
        user_login_form = UserLoginForm()
        context = {'form': user_login_form}
        return render(request,  'userprofile/login.html', context)
    else:
        return HttpResponse("请使用GET或者POST请求数据")
        
# 编写前端页面
vim templates/userprofile/login.html
{% extends "base.html" %} {% load static %}
{% block title %} 登录 {% endblock title %}
{% block content %}
<div class="container">
    <div class="row">
        <div class="col-12">
            <br>
            <form method="post" action=".">
                {% csrf_token %}
                <!-- 账号 -->
                <div class="form-group">
                    <label for="username">账号</label>
                    <input type="text" class="form-control" id="username" name="username">
                </div>
                <!-- 密码 -->
                <div class="form-group">
                    <label for="password">密码</label>
                    <input type="password" class="form-control" id="password" name="password">
                </div>
                <!-- 提交按钮 -->
                <button type="submit" class="btn btn-primary">提交</button>
            </form>
        </div>
    </div>
</div>
{% endblock content %}

# 将登录加到header.html中去
vim templates/header.html
        {% if user.is_authenticated %}
            <li class="nav-item dropdown">
                    {{ user.username }}
                </a>
                <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                    <a class="dropdown-item" href="#">退出登录</a>
                </div>
            </li>
        {% else %}
            <li class="nav-item">
                <a class="nav-link" href="{% url 'userprofile:login' %}">登录</a>
            </li>
        {% endif %}        

# 将路径注册到urls中去
vim userprofile/urls.py
from django.urls import path
from . import views 

app_name='userprofile' 

urlpatterns = [
    path('login/', views.user_login, name='login'),
]

# 将userprofile 路由注册到根路由上(用include)
vim blog/urls.py
urlpattern = [
    # ...
    path('userprofile/', include('userprofile.urls', namespace='userprofile'))
]

# 将userprofile app注册到INSTALLED APP中 
vim blog/settings.py
INSTALLED APPS = [
    # ...
    'userprofile',
]

# userprofile 并没有改动models，因此不用迁移数据

# 修改userprofile/views.py 实现用户登出
from django.contrib.auth import authenticate, login, logout

def user_logout(request):
    logout(request)
    return redirect("articles:articles_list")

# 将路由注册到userproflie/urls.py中 
urlpatterns = [
    # ...
    path('logout/', views.user_logout, name='logout')
]

# 更改header.html中的退出登录链接url
href="{% url 'userprofile:logout' %}"
```

## 用户的注册
```
# 用户注册需要用到表单，所以继续修改userprofile/forms.py
class UserRegisterForm()

# 编写userprofile/views.py 实现注册功能
def user_register(request):
    if request.method == "POST":
        user_register_form = UserRegisterForm(data=request.POST)
        if user_register_form.is_valid():
            new_user = user_register_form.save(commit=False)
            new_user.set_password = (user_register_form.cleaned_data['password'])
            new_user.save()
            login(request, new_user)
            return redirect("articles:articles_list")
        else:
            return HttpResponse("注册表单有误，请重新输入")
    elif request.method == "GET":
        user_register_form = UserRegisterForm()
        context = {'form': user_register_form }
        return render(request, 'userprofile/register.html', context)
    else:
        return HttpResponse("请使用GET或者POST请求数据")

# 注册到 userprofile/urls.py中去
urlpatterns = [
    # ...
    path('register/', views.user_register, name='register'),
]

# 修改前端，增加注册入口
vim templates/userprofile/login.html 

```

## 用户的删除
```
# 用户视图与权限
vim userprofile/views.py
@login_required(login_url='/userprofile/login/')
def user_delete(request, id):
    if request.method == "POST":
        user = User.objects.get(id=id)
        if request.user == user:
            logout(request)
            user.delete()
            return redirect("articles:articles_list")
        else:
            return HttpResponse("没有权限删除")
    else:
        return HttpResponse("仅接受POST请求")
# 编写前端templates
# 添加入口
<a class="dropdown-item" href="#" onclick="user_delete()">删除用户</a>
# 添加弹窗提醒用户
{% if user.is_authenticated %}
            <form 
                    style="display:none;"
                    id="user_delete"
                    action="{% url 'userprofile:delete' user.id %}" 
                    method="POST"
                    >
                    {% csrf_token %}
                    <button type="submit">发送</button>
            </form>
            <script>
                function user_delete(){
                    layer.open({
                        title: "确认删除",
                        content:"确认删除用户资料吗",
                        yes: function(index, layero){
                                 $(`form#user_delete button`).click();
                                 layer.close(index);
                            },
                   })
                }
            </script>
{% endif %}
```

## 密码重置
```
# 在根路由下引入password-reset

# 配置邮箱

```

## 拓展用户信息
```
```

## 上传头像
```
```

## 分页显示
```
```

## 
