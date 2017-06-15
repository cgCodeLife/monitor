# monitor
## 概述：
本项目主要完成一个完整url类型的监控系统搭建，包括监控http状态码、数据接口返回值以及页面元素
主要分为三个部分：web、api、scheduler
欢迎大家批评指正
## web：
使用vue.js开发，实现前端添加删除监控项，查看报警运行记录等功能
## api：
使用php开发，完成数据备份至数据库，与前端+scheduler后端交互等功能
## scheduler
使用python开发，基于apscheduler完成任务的定时执行与是否满足配置条件的扫描

## 环境部署：
web与api的部分需布在同一个nginx的server下，具体nginx配置如下：<br>
server {
    listen              xxxx;
    server_name         xxxx.xxxxxx.xxx;
    more_set_headers    'Server: Apache';
    set $php_upstream 'xxxxxxxxxxx';

    location ~ ^/(favicon.ico|statici|dist) {
        root            /home/work/odp/webroot;
    }

    location ~ \.php$ {
        root            /home/work/odp/webroot;
        fastcgi_pass    $php_upstream;
        fastcgi_index   index.php;
        include         fastcgi.conf;
    }

    location / {
        root /home/work/odp/webroot;
        index index.php;
        fastcgi_pass    $php_upstream;
        include         fastcgi.conf;
        rewrite ^/([^/.]*)(/[^\?]*)?((\?.*)?)$ /$1/index.php$2$3 break;
    }

    location ~ (.*)\.(html|htm)?$ {
         rewrite (.*\.html)$ /index.php;
    }
}

scheduler为独立部署，独立开出一个端口供api进行访问，scheduler所使用的python需安装MysqlDB和ApScheduler的扩展，scheduler的配置文件如下：
[network]
#服务监听端口
port=xxxx
[log]
#日志输出路径，可以指定绝对路径，未指定绝对路径则以当前路径作为相对路径
path=logs
#访问日志
infolog=scheduler.log
#错误和警告日志
warnlog=scheduler.log.wf
[async]
#是否使用协程处理IO，若是则忽略下面对IO线程数的设置
coroutine=true
#允许的IO线程数
io_number=16
#允许的工作线程个数
work_number=32
[scheduler]
#线程个数
thread_num=xxxx
#进程个数
process_num=xxxx
[database]
#数据库主机地址
host=xxxxx
#访问端口
port=xxxx
#用户名
username=xxxx
#密码
password=xxxxx
#数据库名
db=xxxxx
#操作字符集
charset=utf8
[alert]
#这一部分为公司内部发送邮件和短信的通用服务的url，若没有类似服务需实现短信和邮件发送的功能
#邮件url
mailurl=http://xxxxxxxxxxxx
#短信url
messageurl=http://xxxxxxxxxxx
[proxy]
#公司内部访问外网所用的代理
#http代理
http=http://xxxxxxxxx
#https代理
https=http://xxxxxxxxx
启动方式为：nohup /home/work/python/bin/python scheduler_start.py > /dev/null &

## copyright
taihedeveloper全体成员
