# mysite
django2.0入门项目
网站：https://www.djangoproject.com/
web framework
Django was invented to meet fast-moving newsroom deadlines, while satisfying the tough requirements of experiences Web developers.
开发快到离谱；令人放心的安全；可拓展性强
python3.6(https://www.python.org/)+ django2.0(https://www.djangoproject.com/download/)
下载并且安装好python环境和路径后，直接在命令行窗口pip install Django==2.0安装Django2.0，然后用pip list检查当前安装的列表
入门仪式：创建项目，输出Hello，World
    创建项目命令：django-admin startproject <项目名>
    django-admin startproject mysite 文件里面生成的manage.py是管理整个窗口项目的文件
    __init__.py是一个python包
    settings.py 整个窗口项目的设置文件
    urls.py 整个网站的路由文件，规定哪些url可以访问，若没有在规定里面设置网址，那么通过浏览器访问这些网址是访问不了的，只有访问这个文件中规定的网址才可以访问
    wsgi.py网站部署用到的文件
    总结：Django项目基本结构
    Django是一个web框架，所以在网页张输出一个内容，需要涉及到网站，最基本的http协议
    响应请求（客户端是我们打开浏览器的地方，后面urls和views是服务器响应的地方）
    客户端输入一个网址，这时候浏览器就会放松一个请求，发送给服务器，跟服务器说，“我要打开这个网址，你把这个网址里面的东西给我，我要显示出来”，urls文件：规定那些网址有效，那些无效；若有效，进一步处理请求，views将相关内容整理好响应请求，返回内容给客户端。包含http协议：客户端发送请求，服务端响应请求返回给客户端。
    urlpattern=[] 列表，规定那些网址有效；'admin/'：www.baidu.com/admin/; ''空字符串，不用写什么东西；第二个参数代表访问什么东西给客户端，可以写一个专门响应空字符串请求的方法views.py。
    views.py
        from django.http import HttpResponse
        def index(request):#在这个方法中，request参数是固定的
            return HttpResponse("Hello,World")#返回一个响应，这个响应的内容是字符串中的内容，它就封装在HttpResponse中，已经封装好的一个框架，直接使用HttpResponse函数返回我们的相应内容。
    然后在urls.py中调用这个函数,先要导入这个函数的包
    from . import,“.”  代表使用相对路径导入，即从当前项目中寻找需要导入的包或函数
    这时，网址规定（urls.py中的path）好了，处理请求和响应的方法也规定好了(views.py)，然后执行
    执行首先要 启动本地服务 ：在mysite路径打开cmd.exe，输入python manage.py runserver ，里面可以看到网址，然后在浏览器输入网址
    完成
总结
    django2.0 的url用的path，而不是url，也不是re_path（正则表达式，必须加正则表达式，^$）
    urls.py中的admin是后台管理网址,可以设置用户名和密码，之前没有设置过，所以需要命令行设置，那么如何设置呢？可以用帮助命令查询设置语句：python manage.py help；
    createsuperuser创建管理员，怎么创建，继续查看帮助文档python manage.py createsuperuser
    报错（因为这个用户创建必须基于数据库）
    按照提示输入python manage.py migrate就可以把数据库这些文件执行了，数据库文件同步数据库
    数据库在哪里呢?根目录下db.sqlite3是数据库文件
    然后就可以创建用户了createsuperuser(ljb S20200657)
    然后可以浏览器看admin界面啦！
    总结命令：runserver:启动服务器；migrate:同步数据库；createsuperuser创建用户；127.0.0.1相当于localhost本地主机
    帮助命令 help
    基于根目录下cmd 窗口：python manage.py +命令
如果页面比较多（相似内容很多-》抽出来模型，可以将不同点提取出来，标题和内容放到数据库中，用的时候出来，就不用因为标题不同写多个不同函数了）
    创建一个新闻应用 python manage.py startapp 应用名称（这儿命名为article）
    新创建的应用里面有一个models.py 就是上面分析所对应的模型
    编辑models.py创建上面所提到的模型
    这个模型包含两个字段：一个title(CharField(max_length=30) 一个字符串类型的字段，字段长度为30)，一个content（TextField就是一个文本字段,长度比title长，不设置长度）
    自此，只不过是写了一个模型。还需要同步到数据库
        makemigrations ->制造迁移
        migrate ->迁移
    没有改变检测到->是因为虽然创建了这个应用，但是也不能识别就是用了这个新建的article应用,必须得在mysite中settings中设置我们要应用这个应用
    找到installed_apps的列表，添加这个应用'article'
    重新执行此命令，成功创建Article模型，新创建的这个文件实际上是窗口的一套机制，是数据库迁移的产物，当这套代码放到另一个服务器，就可以迅速用这个文件打印一个数据库结构出来，这时候还不是真正的一个表，还有一步：将迁移文件生成一个表
    python manage.py migrate就执行迁移的应用了
    迁移成功后，就把这个文章对应的数据库表创建出来了
    那么我们启动本地服务  看一下把
    如何把新创建的应用在admin中显示出来了？--可以改变article中的admin.py
        from .models import Article
        # Register your models here.
        admin.site.register(Article)
    那么就把自己定义的新闻模块添加到admin，点击进去就可以管理添加了（真神奇，之前我看着还好奇，有了模块，怎么没内容呢，原来在admin后台管理这里添加）
    使用模板显示内容
        我们为什么使用模板呢？就是为了避免使用不同的函数处理不同的链接。 
        我们需要变成一个处理方法可以处理不同的链接
        我们可以看到数据库迁移文件里面不仅有title和content的字段，还有一个自动添加的id字段（唯一标识，是自动字段AutoFiled数字类型，只要添加一个title和content就会生成这个字段的值），里面有一个参数是primary_key是主键的意思，那么主键就是在数据库当中有唯一标识的约束
        views就是写处理函数的文件，写完以后可以简单的链接到路由url里面，因为views中的article_id函数是有参数，所以需要在path路径中指出该参数（默认参数是字符串类型，我们可以定义为int类型，可以有多个网页 path('article/',article_detail,name="article_detail")，可以在路径中给出别名name
        怎么把之前的model模板显示出来,在views中把这个模板引用进来
那么怎么将views.py中的article_id和models.py中Article对应起来？
    objects---模型的objects是获取或操作模型的对象
    Article.objects.get(条件) //    article =Article.objects.get(id=article_id)
    Article.objects.all()
    Article.objects.filter(条件)
        获取文章标题：    return HttpResponse("文章标题: %s" % article.title)
            return HttpResponse("文章标题: %s 文章内容： %s" % (article.title,article.content))
        换行  return HttpResponse("文章标题: %s  <br>  文章内容： %s" % (article.title,article.content))
        增加样式 return HttpResponse("<h2>文章标题: %s</h2> <br>文章内容： %s" % (article.title,article.content))
当超出已有的id范围，应该如何处理？毕竟浏览器是在客户手中用的
    利用try :   article =Article.objects.get(id=article_id) except Article.DoesNotExist: return HttpResponse("不存在")
    另外一种方法，利用Http404,直接返回 404.直接返回时作为错误抛出，还可以
    raise Http404("no page")
自此，最基本的利用模板显示内容已经做好了，但是还是存在一定的问题，比如返回内容 的样式和后端进行混淆的，所以需要分离开来，方便前后端代码改变和操作
     使用模板 将 前端页面和后端代码分离-以降低耦合性
        创建文件夹templates  
        templates已经在settings.py中规定好了        'APP_DIRS': True,说明app中模板文件是有效的
        在templates文件夹内创建 html，然后将views中展示的内容设置好。里面涉及模板的使用
        from django.shortcuts import render  //render是使用模板的东西
        1 利用render返回   来获取模板的内容
        render(request,模板的名字（eg: article_detail.html）,字典（需要自己创建一个字典eg: context=[] context['article_obj']=article）)
        然后再article_detail.html中将设置的字典和内容对应起来<h2>{{article_obj.title}}</h2> <p>{{article_obj.content}}</p>
        在标题和内容间可加<hr>标签---横线
        2 还可以用render_to_response是render的简化，只有两个参数，没有request参数
        3 还有一种方式可以更一步进行优化代码。也是shortcut中的，import get_object_or_404简化try except直接使用两个参数 模型和条件
            article =get_object_or_404(Article,pk=article_id)   pk是主键缩写，因为只会而不知道他的名字，所以直接写成主键缩写
自此，基本以及优化已经完成，那么对于用户来说，访问id方便不方便额？答案是不方便的，那么应该如何做呢？
    文章列表来解决--专门要创建一个文章列表方法来solve
    获取文章列表，即获取全部的文章，用objects.all()
    得到以后，用集合或者字典收集
    对应相应的页面article_list.html
    得到上诉结果是不行的，需要在article_list中利用for循环把它列出来{% for article in articles %}  <p>{{article.title}}</p>{% endfor %}接下来就是实现点击然后跳转页面了
    <a href="/article/{{article.pk}}">{{article.title}}</a>
    也可以用另一种写法：和url设置中的地址对应<a href="{% url 'article_detail' article.pk %}">{{article.title}}</a>
自此，完美实现跳转链接
还要补充一点：总urls包含app的urls，比较松散，写到一哥urls文件中，可以类似admin的urls路由管理文件
即把urls路由文件中的url剪切到article的app的路由管理文件中
在article中新建一个urls.py文件进行路由管理，形式如同urls文件格式
然后在总urls文件中进行include包含    
from django.urls import path,include
path('article/',include('article.urls'))
然后因为总路由文件中以article/开始，所以修改article路由文件中路径
完美~~
