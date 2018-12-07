[TOC]
## supervisor （进程管理工具）
###### http://www.supervisord.org (官方文档,建议阅读)
#### 1.如何用最简单的方法安装supervisor
----

>安装pip（如果你本地环境没有pip）

---
	yum -y install python-pip

>通过pip安装supervisor

---
	pip install supervisor

>通过supervisor的自带命令复制配置文件

---
	echo_supervisord_conf > /etc/supervisord.conf

	/etc/supervisord.conf 为supervisor配置文件的路径,视情况可放在别的目录

#### 2.配置文件详解
----

>主配置文件/etc/supervisord.conf

---
	最重要的为include标签,主要作用为当有很多进程需要supervisor管理时，
	可以通过include的方法来进行文件导入这样就可以轻松的管理supervisor

>子配置文件*.ini 或者 *.conf 

----
	supervisor的配置文件可分为.ini格式或者.conf格式，具体内容没有区别

	[program:admama_active]   进程名称(admama_active)
	command=/usr/bin/php bin/console ADMAMA:endless_command ACTIVE  主要执行的脚本命令
	directory=/data/http/AD-API/                                    脚本所在的目录
	user=root                                                       执行的用户
	autostart=true                                                  是否自动启动
	autorestart=true                                                是否在关闭后自动重启
	startsecs=3                                                     成功之后所等待的时间（单位：秒）
	stdout_logfile=/data/supervisor/log/active.log                  正常日志输出目录
	stderr_logfile=/data/supervisor/log/active.log                  错误日志输出目录

#### 3.supervisor的运行方法
----
	1.supervisord -c /etc/supervisord.conf 加载配置文件,连带启动supervisord
	2.supervisorctl reread (当配置文件发生更改的时候，重新读取最新的配置文件)
	3.supervisorctl reload(重启supervisorctl)
	4.supervisorctl status (查看各个脚本的执行情况)
    5.其余类似stop start 等命令，请自行查看supervisord官方文档

#### 4.坑点
----
	1.请尽可能的安装最新版本的supervisor 低版本的supervisor会有各种bug产生。
	2.所执行的脚本本身需要进行死循环来阻止脚本本身执行完成。
	如果supervisor监听到脚本执行完成后会自动判断脚本已经挂掉，然后就会立刻重启。
	当重启的次数超过设置的值时，supervisor则会放弃继续开启脚本，从而退出
	所以supervisor适合的场景可以用于websocket等需要长时间执行且不需要关闭的脚本
