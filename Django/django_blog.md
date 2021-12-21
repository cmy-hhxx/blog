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
