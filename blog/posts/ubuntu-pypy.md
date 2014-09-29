#ubuntu下部署 virtualenv 和 pypy 和mysqldb
>2014/8/12
#1 安装系统环境

	apt-get install mysql python-dev
#2 安装 virtualenv

	easy_install virtualenv
#3 安装pypy

	apt-get install pypy
#4 创建基本环境

	cd /www/web
	virtualenv --no-site-packages -p env-pypy
	cd env-pypy
	source bin/activate

	#把源文件放进来

	pip install -r requirement.txt

	mkdir etc
	echo_supervisord_conf > etc/supervisord.conf

	vi etc/supervisord.conf
	#添加配置
	[program:app]
	command=python app.py
	autostart=true
	stderr_logfile = /var/log/nginx/app.log
	stdout_logfile = /var/log/nginx/app.log

	supervisord -c etc/supervisord.conf

#5 使用pymysql

由于mysqldb在pypy下有问题，这里选用了pymysql，可以代替mysqldb

	try:
	    import pymysql
	    pymysql.install_as_MySQLdb()
	except:
	    pass
#6 安装mysqldb
	mysqldb依赖

	sudo apt-install mysql-dev pypy-dev