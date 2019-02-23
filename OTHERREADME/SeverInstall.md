# 服务器上安装

**一、更新系统软件包**

```
yum update -y
```

**二、安装软件管理包和可能使用的依赖**

```python
yum -y groupinstall "Development tools"
yum install openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel psmisc
```

**三、下载Pyhton3到/usr/local 目录** 

```
cd /usr/local
wget https://www.python.org/ftp/python/3.6.6/Python-3.6.6.tgz
```

解压

```
tar -zxvf Python-3.6.6.tgz
```

进入 Python-3.6.6路径

```
cd Python-3.6.6
```

编译安装到指定路径

```
./configure --prefix=/usr/local/python3
```

**注意：这里的 prefix 路径是可以改变的，自己记着就行，下边要用到。**

开始编译和安装

```
make
make install
```

安装完成之后 建立软链 添加变量 方便在终端中直接使用 python3 和 pip 工具**如果你上面自定义了路径这里请更改路径**

```
ln -s /usr/local/python3/bin/python3.6 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3.6 /usr/bin/pip3
```

**四、查看Python3和pip3安装情况**

![timg.jpg](https://www.django.cn/media/upimg/timg_20180709220813_231.jpg)

**五、安装virtualenv ，建议大家都安装一个virtualenv，方便不同版本项目管理。**

```
pip3 install virtualenv
```

建立软链接

```
ln -s /usr/local/python3/bin/virtualenv /usr/bin/virtualenv
```

安装成功在根目录下建立两个文件夹，主要用于存放env和网站文件的。(个人习惯，其它人可根据自己的实际情况处理)

```
mkdir -p /data/env
mkdir -p /data/wwwroot
```

**六、切换到/data/env/下，创建指定版本的虚拟环境。**

```
virtualenv --python=/usr/bin/python3 pyweb
```

然后进入/data/env/pyweb/bin 
启动虚拟环境：

```
source activate
```

![timg.jpg](https://www.django.cn/media/upimg/timg_20180709220840_146.jpg)

留意我标记的位置，出现(pyweb)，说明是成功进入虚拟环境。

**七、虚拟环境里用pip3安django和uwsgi**

```
pip3 install django （如果用于生产的话，则需要指定安装和你项目相同的版本）
pip3 install uwsgi
```

**留意：** ** uwsgi 要安装两次**，先在系统里安装一次，然后进入对应的虚拟环境安装一次。

给 uwsgi 建立软链接，方便使用

```
ln -s /usr/local/python3/bin/uwsgi /usr/bin/uwsgi
```

**八、下载博客项目文件**

进入到 wwwroot 目录放置项目

```
cd /data/wwwroot/
```

通过 git 下载项目

```
git clone https://github.com/jasonbanjui/tonight-blog.git
```

**九、安装 mysql 数据库**

请移步到[这里](https://www.cnblogs.com/xiaopotian/p/8196464.html)查看 Centos7通过yum安装最新MySQL

**十、测试项目是否能够运行**

```
python3 manage.py runserver
```

![timg.jpg](https://www.django.cn/media/upimg/timg_20180709221156_198.jpg)
正常运行！

**如果项目无法运行请发送问题截图发送到邮箱 794130574@qq.com**

**十一、Django正常运行之后我们就开始配置一下uwsgi。**

我们网站项目路径是 /data/wwwroot/tonight-blog/,在项目根目录下创建
blog.xml文件，输入如下内容：

```
<uwsgi>    
   <socket>127.0.0.1:8997</socket> <!-- 内部端口，自定义 --> 
   <chdir>/data/wwwroot/tonight-blog/</chdir> <!-- 项目路径 -->            
   <module>blog.wsgi</module>  <!-- mysite为wsgi.py所在目录名--> 
   <processes>4</processes> <!-- 进程数 -->     
   <daemonize>uwsgi.log</daemonize> <!-- 日志文件 -->
</uwsgi>
```

保存

**十二、安装 nginx 和配置 nginx.conf 文件**

安装所需环境

```
yum install gcc-c++
yum install -y pcre pcre-devel
yum install -y zlib zlib-devel
yum install -y openssl openssl-devel
```

下载 nginx 

```
wget -c https://nginx.org/download/nginx-1.10.1.tar.gz
```

解压

```
tar -zxvf nginx-1.10.1.tar.gz
cd nginx-1.10.1
```

配置

```
./configure
```

编译安装

```
make
make install
```

nginx一般默认安装好的路径为/usr/local/nginx
在/usr/local/nginx/conf/中先备份一下nginx.conf文件，以防意外。

```
cp nginx.conf nginx.conf.bak
```

然后打开nginx.conf，把原来的内容删除，直接加入以下内容：

```
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    server {
        listen 80;
        server_name  www.django.cn; #改为自己的域名，没域名修改为127.0.0.1:80
        charset utf-8;
        location / {
           include uwsgi_params;
           uwsgi_pass 127.0.0.1:8997;  #端口要和uwsgi里配置的一样
           uwsgi_param UWSGI_SCRIPT blog.wsgi;  #wsgi.py所在的目录名+.wsgi
           uwsgi_param UWSGI_CHDIR /data/wwwroot/tonight-blog/; #项目路径
           
        }
        location /static/ {
        alias /data/wwwroot/tonight-blog/static/; #静态资源路径
        }
    }
}
```

 要留意备注的地方，要和UWSGI配置文件blog.xml，还有项目路径对应上。 
进入/usr/local/nginx/sbin/目录
**执行 ./nginx -t 命令先检查配置文件是否有错，没有错就执行以下命令：**

```
./nginx
```

终端没有任何提示就证明nginx启动成功。可以使用你的服务器地址查看，成功之后就会看到一个nginx欢迎页面。

之后，在settings.py里设置：

1、关闭DEBUG模式。

DEBUG = False  

2、ALLOWED_HOSTS设置为* 表示任何IP都可以访问网站。

ALLOWED_HOSTS = ['*']

**十三、访问项目的页面。**
进入网站项目目录

```
cd /data/wwwroot/tonight-blog/
```

执行下面命令：

```
uwsgi -x mysite.xml
```

以上步骤都没有出错的话。
进入/usr/local/nginx/sbin/目录
执行：

```
./nginx -s reload
```

重启nginx 。



参考文章：[CentOS7下部署Django项目详细操作步骤](https://www.django.cn/article/show-4.html#buzhou)