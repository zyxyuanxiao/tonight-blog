# tonight-blog
django2.1 + xadmin2.0 + 杨青个人博客模板 《今夕何夕》打造的一款__美观，安全，稳定__的博客

###### README（解释文件）、READMEIMAGES（解释文件中图片的位置）和 OTHERREADME（其他说明） 都不是这个项目的文件，不需要可以删除

更新日记请点[这里](https://github.com/jasonbanjui/gentle-blog/OTHERREADME/UpdateFile.md)

# 开始

### 1.安装（mysql的安装这里不演示，mysql的版本要> = 4.5）

需要安装python3.6并通过pip安装django和xadmin（见1.1）

```
 pip install django-crispy-forms
 pip install django-formtools 
 pip install django-import-export
 pip install django-simple-captcha
 pip install django-pure-pagination
 pip install pillow
 pip install django
 pip install django-crispy-forms
 pip install django-import-export
 pip install django-reversion
 pip install django-formtools
 pip install django-ckeditor
 pip install future
 pip install httplib2
 pip install six
 pip install mysqlclient
```

#### 1.1安装xadmin

- ###### 下载xadmin

  - git用户

  `git clone https://github.com/sshwsfc/xadmin.git`

  如果有ssh

  `git clone git@github.com:sshwsfc/xadmin.git`

  **克隆到本地之后需要打包成拉链**

  - 其他用户

  访问直接<https://github.com/sshwsfc/xadmin>下载

  [![1550461969086](https://github.com/derknight/gentle-blog/raw/master/READMEIMAGES/1550461969086.png)](https://github.com/derknight/gentle-blog/blob/master/READMEIMAGES/1550461969086.png)

  [![1550461997891](https://github.com/derknight/gentle-blog/raw/master/READMEIMAGES/1550461997891.png)](https://github.com/derknight/gentle-blog/blob/master/READMEIMAGES/1550461997891.png)

- ###### pip安装xadmin

  1. **确保文件在文件加下后，在地址栏上输入cmd打开cmd**

  [![1550461582701](https://github.com/derknight/gentle-blog/raw/master/READMEIMAGES/1550461582701.png)](https://github.com/derknight/gentle-blog/blob/master/READMEIMAGES/1550461582701.png)

  2. **命令行pip安装**

  [![1550461764735](https://github.com/derknight/gentle-blog/raw/master/READMEIMAGES/1550461764735.png)](https://github.com/derknight/gentle-blog/blob/master/READMEIMAGES/1550461764735.png)

  代码 `pip install xadmin-django2.zip`

  看到成就就可以了

### 使用

在项目路径打开cmd输入以下命令

```
python manage makemigrations
python manage migrate
python manage createsuperuser
python manage runserver 8080
```

代码解释：

1. 检查数据表是否有更新
2. 生成数据表
3. 创建超级用户
4. 在8080端口运行服务
5. 输入完后通过URL **localhost：8080 **访问

[服务器安装教程](https://github.com/jasonbanjui/gentle-blog/OTHERREADME/SeverInstall.md)

**如果有中间的疑问请话教育邮箱发送到794130574@qq.com**

# 开发者文档

### 页面

- base基础模板页面（导航栏+底部）
- index首页
- article文章页面
- list文章类型×文章标签×关键字搜索
- 评论留言

### 应用

- MainCoreApp博客的重要部分
  1. model表单设计
  2. adminx注册后台表单
  3. url路由
  4. form设计表单验证
  5. views业务逻辑
- CommonComment留言模块

### 2.1 404和500页面自定义

将404和500的页面替换为MainCoreApp中模板文件夹中的404和500的html

[![1550463277665](https://github.com/derknight/gentle-blog/raw/master/READMEIMAGES/1550463277665.png)](https://github.com/derknight/gentle-blog/blob/master/READMEIMAGES/1550463277665.png)

### 2.2更换表单在后台显示的字段等自定义（建议配合models.py文件查看）

1. 只需要修改MainCoreApp中的adminx文件即可

一些常见的字段

```
list_display # 显示字段 （值：<list:字段>）
search_fields # 可以被查找的字段 （值：<list:字段>）
list_filter # 使用过滤器 （值：<list:字段>）
model_icon # 使用图标 样式是 font-awesome 的 （值：<str:样式>）
ordering # 排序 （字段前加 - 是倒序，比如 ['-name']） （值：<list:字段>）
readonly_fields # 设置只读字段（就是不可通过 xadmin 后台修改） （值：<list:字段>）
relfield_style # 一次性不会全部加载所有内容，比如说下拉框 （一般是 fk-ajax）
list_editable # 设置某些字段在表单列表中直接可以编辑 （值：<list:字段>）
refresh_times # 设置提供几秒刷新页面 （值：<list:秒数>）
```

1. 修改

比如是想修改StromInfo这个表单就在xadmin.py中找到StromInfoAdmin类

[![1550463664784](https://github.com/derknight/gentle-blog/raw/master/READMEIMAGES/1550463664784.png)](https://github.com/derknight/gentle-blog/blob/master/READMEIMAGES/1550463664784.png)

**改写其中的字段以及对应的值即可**

### 2.3自定义后台全局设置（比如左上角的标题，底部的内容，是否开启主题功能）

只需要修改MainCoreApp中的adminx文件即可

__主要的是BaseSetting和GlobalSettings __

```
class  BaseSetting（object）：
    enable_themes =  True  ＃开启主题功能 
    use_bootswatch =  True  ＃开启主题选择功能 
    menu_style =  ' accordion '  ＃ APP折叠
    
    
class  GlobalSettings（object）：
    site_title =  “ XXX管理系统”  ＃改标题 
    site_footer =  “ XXX网”  ＃底部版权 
    menu_style =  “ accordion ”  ＃收汇导航栏
```

未待完续....