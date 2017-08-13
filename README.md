yum -y install git 

git  clone https://github.com/chaishaohua/zsky.git

cd zsky&&sh zsky.sh


运行爬虫

cd /root/zsky

nohup python simdht_worker.py>/root/zsky/spider.log 2>&1&


杀死爬虫

ps -ef|grep simdht_worker.py|grep -v grep|awk '{print $2}'|xargs kill -9


重启爬虫

ps -ef|grep simdht_worker.py|grep -v grep|awk '{print $2}'|xargs kill -9

cd /root/zsky

nohup python simdht_worker.py>/root/zsky/spider.log 2>&1&



立即重启

shutdown -r now

一键检测服务器配置、IO、下载速度

wget -qO- bench.sh | bash

是否有Swap

free -m

查找python进程是否有运行

ps -ef|grep gunicorn

ps -ef|grep simdht|awk '{print $2}'|grep -v grep|xargs ps hH |wc -l

或

ps -xH|grep simdht|grep -v grep|wc -l

查看爬虫当前线程数

进入 zsky 目录

cd /root/zsky

python运行 爬虫脚本

python simdht_worker.py

cpu在跑 后台数据不增加 运行一下命令

cd /root/zsky

sh zsky-reboot.sh


空密码登入数据库

mysql -u root

设置root的密码为123456

set password for 'root'@'localhost'=password('123456');


删除爬取统计

进入mysql

mysql

进入指定数据库

use zsky;

清空zsky库下的search_statusreport表数据

delete from search_statusreport;

退出mysql数据库

exit;

索引命令

/usr/local/sphinx-jieba/bin/indexer -c /root/zsky/sphinx.conf film --rotate


访问网站出现nginx error!

cd /root/zsky

nohup gunicorn -k gevent --access-logfile zsky.log --error-logfile zsky_err.log  manage:app -b 0.0.0.0:8000 -w 4 --reload>/dev/zero 2>&1&


搜索页打不开，出现 Internal Server Error

进入 sphinx 缓存目录

cd /usr/local/sphinx-jieba/

重建sphinx搜索缓存

/usr/local/sphinx-jieba/bin/indexer -c /root/zsky/sphinx.conf film --rotate&&/usr/local/sphinx-jieba/bin/searchd --config ~/zsky/sphinx.conf
