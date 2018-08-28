### 初次学习环境准备

计划运行环境：
 
操作系统：ubuntu 
代理服务器：Nginx
应用服务器：uwsgi
web 应用框架：flask 
缓存：Redis
开发语言： python 2.7
数据库 SqlServer

查了一些资料，发现使用Python开发过程中，会遇到依赖第三方组件包问题，每个应用可能需要各自拥有一套“独立”的Python运行环境
考虑碰到 python2.7 和python 3.* 兼容问题也比较麻烦，
所以准备之前就想好在什么样的环境下进行配置

背景：
计划开发一个API接口访问地址，通过该地址可以直接登录系统，
API 要完整的事情如下：
验证请求是否合法，合法后验证系统中是否有报名信息，如果有，则生成可识别的Token并且可操作Token推送Redis中，直接跳转到系统主页即可


step 1  应用环境设置

	python2.7 虚拟环境：
	
	sudo apt-get install virtualenv  --安装组件
	mikdir my_project
	cd my_project
	virtualenv venv --创建环境   
	virtualenv --no-site-packages venv  --不带任何第三方包的“干净”的Python运行环境。

	source venv/bin/activate --进入激活环境   . venv/bin/activate

	命令提示符行出现变化：
	（venv）  root@xxxx:/xx/xx/xx#

	pip list 查看已安装的第三方包  

	--在venv环境下，用pip安装的包都被安装到venv这个环境下，系统Python环境不受任何影响。也就是说，venv环境是专门针对myproject这个应用创建的。

	pip install Jinja2
	pip install --upgrade pip
	pip install uwsgi
	pip install flask
	pip install redis
	pip install requests

	source venv/bin/deactivate --退出环境 


	python3.* 虚拟环境：
	pip3 install virtualenv

	略，可参考网络资源

	不同应用间多版本的冲突问题可使用virtualenv 来解决隔离问题


step2 nginx + uwsgi + flask

	(1) nginx
		sudo apt-get install nginx 

		所有的配置文件都在/etc/nginx下，并且每个虚拟主机已经安排在了/etc/nginx/sites-available下
		程序文件在/usr/sbin/nginx
		日志放在了/var/log/nginx中
		并已经在/etc/init.d/下创建了启动脚本nginx
		默认的虚拟主机的目录设置在了/var/www/nginx-default (有的版本 默认的虚拟主机的目录设置在了/var/www, 请参考/etc/nginx/sites-available里的配置)

		启动nginx
		sudo /etc/init.d/nginx start
		sudo /etc/init.d/nginx restart
		sudo /etc/init.d/nginx stop
 		# service nginx {start|stop|status|restart|reload|configtest|}

		nginx 配置文件说明：

		access_log  /home/xxx/xxx/access.log; --配置访问日志指定路径
		error_log /home/xxx/xxx/error.log;--配置访问报错后分析日志，大部分Nginx配置错误可以在这里查看，修改配置后重启Nginx restart

		location / {
				include uwsgi_params; --与 uwsgi 结合使用必须配置项
                uwsgi_pass      127.0.0.1:9091; --与uwsgi 结合使用必须配置项，指定uWsgi可访问的内部端口地址，协议：socket

                uwsgi_param UWSGI_CHDIR  /home/xxx/login_demo;-- 指定目录
                uwsgi_param UWSGI_SCRIPT hello:app;-- 指定应用

		}

		部署多个应用参考 
			location /myapp {
			    include uwsgi_params;
			    uwsgi_param SCRIPT_NAME /myapp;  ----uWSGI参数”SCRIPT_NAME”，值为应用的路径”/myapp”
			    uwsgi_pass 127.0.0.1:3031;
			}


		其他配置可参考网络资源



		ps -aux | grep nginx --查看进程状态,经常使用，牢记
		ps -ef | grep nginx --查看进程状态,经常使用，牢记

		netstat -ap | grep nginx --找出程序运行的端口  找出运行在指定端口的进程 # netstat -an | grep ':80'

		例如： kill -9 [PID] --kill 命令用于终止进程
		通常用 ps 查看进程 PID ，用 kill 命令终止进程

		netstat -a  --列出所有端口 包括监听和未监听的  只列出所有监听 tcp 端口 netstat -lt  只显示监听端口 netstat -l
		netstat -at --列出所有 tcp 端口 与外部系统交互时，可以查看是否可连接 显示 TCP 或 UDP 端口的统计信息 netstat -st 或 -su
		netstat -au --列出所有 udp 端口  只列出所有监听 udp 端口 netstat -lu 

		awk '{print $1}' access.log |sort|uniq -c|sort -nr|head -10  ------分析access.log获得访问前10位的ip地址 - 
		netstat -nat | grep "192.168.1.15:22" |awk '{print $5}'|awk -F: '{print $1}'|sort|uniq -c|sort -nr|head -20
		-----查看连接某服务端口最多的的IP地址


		fuser -k 9090/tcp  杀掉进程9090端口，重新运行 暴力方式

	（2）uwsgi

		uwsgi --http-socket :9091 --plugin python --wsgi-file testuwsgi.py --脚本运行实例参考

		创建uwsgi环境:
		

		vim uwsgi_config.ini --脚本命令不好记，可转成配置文件进行加载（ini,xml)uwsgi的启动可以把参数加载命令行中，也可以是配置文件 .ini, .xml, .yaml 配置文件中 uwsgi --help

			-------------源码安装方式
			# 可以去pypi，搜索uwsgi下载：
			https://pypi.python.org/pypi/uWSGI/ 
			# 安装命令如下：
			tar xvzf uwsgi-2.0.9.tar.gz
			cd uwsgi-2.0.9
			make
			---------------

		eg:
		------------------------------------------------------------------------------------
		[uwsgi]
		socket = 127.0.0.1:9091         --用nginx的时候就配socket , 直接运行的时候配 http
		chdir = /home/xxx/login_demo  --指定项目的目录
		pythonpath = /home/xxx/login_demo
		#module = hello   --module          = partner.wsgi:application对于- myweb_uwsgi ni文件来说，与它的平级的有一个partner目录，这个目录下有一个wsgi.py文件
		#plugin =  python
		wsgi-file = /home/xxx/login_demo/hello.py -- 部署多个应用指定路径时要注释掉
		callable = app  --指定应用对象 Flask 的名称 如 app
		processes = 4  --开启的进程数量
		threads = 2
		vacuum  = true-- 当服务器退出的时候自动清理环境，删除unix socket文件和pid文件
		stats = %(chdir)/uwsgi/uwsgi.status
		pidfile = %(chdir)/uwsgi/uwsgi.pid   
		daemonize = /home/xxx/login_demo/server.log -- uwsgi 运行日志，查看错误内容
		virtualenv＝/home/xxx/virtualenv--虚拟环境来避免应用间冲突

		mount=/myapp=server.py 
		manage-script-name=true
		--“mount”参数表示将”/myapp”地址路由到”server.py”中，”manage-script-name”参数表示启用之前在Nginx里配置的”SCRIPT_NAME”参数。再次重启Nginx和uWSGI，部署多个应用参考
		-------------------------------------------------------------------------------------
		uwsgi --ini myweb_uwsgi.ini --运行uwsgi

		venv/bin/uwsgi uwsgi_config.ini --启动 如果安装的uwsgi是装在系统环境下面，需要运行时指定虚拟环境依赖包下的uwsgi运行才可以

		uwsgi --reload uwsgi/uwsgi.pid --重启，配置uwsgi后，可单独指定uwsgi.pid，Python文件修改后，可直接重启查看运行结果


	（3）flask

			*.py文件基本用法
			------------------------------
			from flask import Flask
			app = Flask(__name__) --callable = app
			 
			@app.route('/')
			def index():
			    return '<h1>Hello World</h1>'


		详请参考网络相关配置



	（4）redis

		sudo apt-get redis-server --安装服务器端，设置后可做为存储库，默认端口：6379

		find / -name "redis.conf" --查找配置文件，默认安装时，经常找不到配置文件路径

		sudo /etc/init.d/redis-server restart (不重启,你修改的配置不会生效)

		logfile /var/log/redis/redis-server.log

		/usr/bin/redis-server 

		可通过桌面redis客户端连接，验证是否正常启动访问



		pip install redis  --安装后Python可以引用进行操作以下封装使用，需要了解掌握各种可支持类型

		---------------------------------------------------
		import redis
		class Redis:
		    @staticmethod
		    def connect():
		        r = redis.StrictRedis(host='172.16.21.2',port=6379,db=0)
		        return r
		    @staticmethod
		    def set_data(r,key,data,ex=None):
		        r.set(key,data,ex)

		    @staticmethod
		    def hset_data(r,name,key,data,ex=None):
		        r.hset(name,key,data)
		    @staticmethod
		    def hget_data(r,name,key):
		        data = r.hget(name,key)
		        if data is None:
		            return None
		        return data

		    @staticmethod
		    def get_data(r,key):
		        data = r.get(key)
		        if data is None:
		          return None
		        return data
		    -------------------------------------------------




创建应用参考目录：


├── hello.py
├── hello.pyc
├── uwsgi
│   ├── uwsgi.log                  # 日志文件，通过该文件查看uwsgi的日志
│   ├── uwsgi.pid                  # pid文件，通过该文件可以控制uwsgi的重启和停止
│   ├── uwsgi.sock                 # socket文件，配置nginx时候使用
│   └── uwsgi.status               # status文件，可以查看uwsgi的运行状态
└── uwsgi.ini

[uwsgi]
chdir=/home/xxx/web/flask/mysite/
home=/home/xxx/web/flask/mysite/.env
module=hello                                 # python文件的名称
callable=app
master=true
processes=2                                  # worker进程个数
chmod-socket=666
logfile-chmod=644
uid=xxx_web
gid=xxx_web
procname-prefix-spaced=mysite                # uwsgi的进程名称前缀
py-autoreload=1                              # py文件修改，自动加载
#http=0.0.0.0:8080                           # 监听端口，测试时候使用

vacuum=true                                  # 退出uwsgi是否清理中间文件，包含pid、sock和status文件
socket=%(chdir)/uwsgi/uwsgi.sock             # socket文件，配置nginx时候使用
stats=%(chdir)/uwsgi/uwsgi.status            # status文件，可以查看uwsgi的运行状态
pidfile=%(chdir)/uwsgi/uwsgi.pid             # pid文件，通过该文件可以控制uwsgi的重启和停止
daemonize=%(chdir)/uwsgi/uwsgi.log           # 日志文件，通过该文件查看uwsgi的日志

uwsgi --ini uwsgi.ini             # 启动
uwsgi --reload uwsgi.pid          # 重启
uwsgi --stop uwsgi.pid            # 关闭

uwsgi --connect-and-read uwsgi/uwsgi.status  #读取uwsgi实时状态

# pip install uwsgitop  #实时动态查看状态 - uwsgitop
# uwsgitop uwsgi/uwsgi.status

--------------------------------------------------------
# -*- coding: utf-8 -*-
from flask import Flask, jsonify, abort, make_response
app = Flask(__name__)
articles = [
    {
        'id': 1,
        'title': 'the way to python',
        'content': 'tuple, list, dict'
    },
    {
        'id': 2,
        'title': 'the way to REST',
        'content': 'GET, POST, PUT'
    }
]
@app.route('/blog/api/articles', methods=['GET'])
def get_articles():
    """ 获取所有文章列表 """
    return jsonify({'articles': articles})
@app.route('/blog/api/articles/<int:article_id>', methods=['GET'])
def get_article(article_id):
    """ 获取某篇文章 """
    article = filter(lambda a: a['id'] == article_id, articles)
    if len(article) == 0:
        abort(404)
    return jsonify({'article': article[0]})
@app.errorhandler(404)
def not_found(error):
    return make_response(jsonify({'error': 'Not found'}), 404)
if __name__ == '__main__':
    app.run(host='127.0.0.1', port=5632, debug=True)
------------------------------------------------

from flask import request
@app.route('/blog/api/articles', methods=['POST'])
def create_article():
    if not request.json or not 'title' in request.json:
        abort(400)
    article = {
        'id': articles[-1]['id'] + 1,
        'title': request.json['title'],
        'content': request.json.get('content', '')
    }
    articles.append(article)
    return jsonify({'article': article}), 201
--------------------------------------
$ curl -i -H "Content-Type: application/json" -X POST -d '{"title":"the way to java"}' http://localhost:5632/blog/api/articles
HTTP/1.0 201 CREATED
Content-Type: application/json
Content-Length: 87
Server: Werkzeug/0.11.4 Python/2.7.11
Date: Wed, 17 Aug 2016 03:07:14 GMT
{
  "article": {
    "content": "",
    "id": 3,
    "title": "the way to java"
  }
}
-----------------------------------------------------
常见 HTTP 状态码
HTTP 状态码主要有以下几类：

1xx —— 元数据
2xx —— 正确的响应
3xx —— 重定向
4xx —— 客户端错误
5xx —— 服务端错误
常见的 HTTP 状态码可见以下表格：

代码	说明
100	Continue。客户端应当继续发送请求。
200	OK。请求已成功，请求所希望的响应头或数据体将随此响应返回。
201	Created。请求成功，并且服务器创建了新的资源。
301	Moved Permanently。请求的网页已永久移动到新位置。 服务器返回此响应（对 GET 或 HEAD 请求的响应）时，会自动将请求者转到新位置。
302	Found。服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。
400	Bad Request。服务器不理解请求的语法。
401	Unauthorized。请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。
403	Forbidden。服务器拒绝请求。
404	Not Found。服务器找不到请求的网页。
500	Internal Server Error。服务器遇到错误，无法完成请求。
curl 命令参考
选项	作用
-X	指定 HTTP 请求方法，如 POST，GET, PUT
-H	指定请求头，例如 Content-type:application/json
-d	指定请求数据
–data-binary	指定发送的文件
-i	显示响应头部信息
-u	指定认证用户名与密码
-v	输出请求头部信息
other： 

	ntpdate time.windows.com (微软公司授时主机(美国)) 
	--对时  


	

