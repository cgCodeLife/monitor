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
## copyright
taihedeveloper全体成员