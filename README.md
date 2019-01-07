# 上海梅花桩web项目

运行demo：  
source activate meihuazhuan
cd /root/meihuazhuang/django/DjangoBlog
python manage.py runserver  0.0.0.0:8000
浏览器打开: http://123.207.166.127:8000/ 就可以看到效果了
代码链接：
环境布置：
centos7+Apache+mysql+Django+python3
python环境布置
Anconda—linux安装
Conda  create meihuazhuan
Source activate meihuazhuan
mysql布置
下载mysql的repo源并安装
#rpm -Uvh http://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
2、修改安装的Mysql版本
(1)List item
查看repo源可安装版本
#yum repolist all | grep mysql
之后会显示可安装的版本列表，默认安装版本为5.7
(2)修改默认版本
# vi /etc/yum.repos.d/mysql-community.repo
打开repo文件，将其中的5.7版本的enabled=1改为enabled=0以及8.0版本的enabled=0改为enabled=1，结果如下
[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql80-community]
name=MySQL 8.0 Community Server
baseurl=http://repo.mysql.com/yum/mysql-8.0-community/el/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
(3)查看可使用版本
yum repolist enabled | grep mysql
3、安装MySQL
#yum install -y mysql-community-server
## 注意，此过程很漫长，需要良好网络
4、启动MySQL并设置开机启动
# 启动
# systemctl start mysqld

# 开机启动
# systemctl enable mysqld
# systemctl daemon-reload
5、生成初始密码
#grep 'temporary password' /var/log/mysqld.log 
6、登录MySQL并修改初始密码

## 登录MySQL
mysql -uroot -p  初始密码

## 修改初始密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Meihua@123456';
#  密码强度默认为中等强度，需要由大写字母、小写字母、特殊符号、数字组成

# 可以先修改密码强度再修改密码
set global validate_password_length=4; (最小值是4，默认为8)
set global validate_password_policy=0
# 选择0（LOW），1（MEDIUM），2（STRONG）其中一种，选择2需要提供密码字典文件
# 之后修改密码便可以使用简单密码
Django安装
开启 virtualenv python3 环境
# source activate meihuazhuan
在此环境安装 Django 相关模块
注意：使用pip安装的内容都需要在虚拟环境下安装，否则将默认安装在CentOS自带的Python2相关环境中
## 安装pymysql库
# pip install django pymysql
安装python相关包需要用到python中的pip命令，比如我项目中需要的包有:

pip install Django
pip install PyMySQL
pip install Scrapy
pip install beautifulsoup4
pip install bs4
pip install lxml
pip install numpy
pip install requests
pip install simplejson
pip install urllib3
Apache布置
1、安装 apache package
# yum install httpd httpd-devel -y
2、安装 mod_wsgi for python3
# pip install mod_wsgi 
注意：这里需要在配置的虚拟环境中安装，不要在外部使用yum install mod_wsgi安装，这将会使mod_wsgi模块默认安装在CentOS7自带的Python2中
3、导出 apache 所需的 mod_wsgi 模块

# mod_wsgi-express install-module

## 结果（根据虚拟目录名称不同，这里结果也略有不同，但大致相同）
## LoadModule wsgi_module "/usr/lib64/httpd/modules/mod_wsgi-py36.cpython-36m-x86_64-linux-gnu.so"
## WSGIPythonHome "/var/www/html/.py3env"
4、配置 apache 配置文件
# vi /etc/httpd/conf/httpd.conf
在最后添加
LoadModule wsgi_module "/usr/lib64/httpd/modules/mod_wsgi-py36.cpython-36m-x86_64-linux-gnu.so"
5 创建相应的Django配置文件
# vi /etc/httpd/conf.d/django.conf
6、重启 apache 并设置开机自启动
# systemctl restart httpd

# systemctl enable httpd

Demo项目：
Cd ~/meihuazhuang
Git clone https://github.com/liangliangyy/DjangoBlog.git
进入到工程中
sudo su root
cd project/django-restframework-demo/tutorial
python manage.py runserver 0.0.0.0:80 &

参考：
https://blog.csdn.net/w18211679321/article/details/83004286
https://blog.csdn.net/a394268045/article/details/79288718
https://github.com/liangliangyy/DjangoBlog
