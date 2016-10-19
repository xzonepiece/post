---
ID: 11117
post_title: 部署到 Heroku
author: admin
post_date: 2016-09-17 00:18:10
post_excerpt: ""
layout: post
permalink: >
  https://www.huginn.cn/deploy/deploy-to-heroku
published: true
---
如果要将 Huginn 部署到 Heroku 平台，我们**推荐你使用最便宜的付费方案**，但是，如果你要使用免费方案的话，你需要注意以下几点：

*   使用 Heroku 免费方案的用户，其每个网站在 30 分钟内无人访问后便会自动关闭，再有人访问时才会自动重新打开，因此为了使 Huginn 网站能长时间运行，可以使用类似 [uptimerobot][1] 的网站监控服务，不停地ping你的 Huginn 网站地址；
*   Heroku 免费方案只允许用户所有的 app 每个月运行的总时长不超过 550 个小时（绑定信用卡的用户可额外再获得 450 个小时），这意味着你无法保证部署好的 Huginn 网站每个月每天都在运行，因此，你可选择绑定信用卡，或者让你的 Huginn 网站每天只允许 18 个小时，这样的话，你的网站每天都可以在运行；
*   Heroku免费方案提供给用户的 Postgres 数据库存储的数据不能超过 10000 行，因此你需要控制 Agent 生成的事件的保留周期，同时限制数据库中 Agent 日志文件的长度，比如 `heroku config:set AGENT_LOG_LENGTH=20`。

### 初次部署到Heroku的操作步骤：

1.  注册 [Heroku][2] 平台账号，然后下载安装 [Heroku Toolbelt][3]；
2.  远程登录 Heroku： `heroku login`
3.  创建 app：`heroku create xxx`（xxx 为 app 的名字，下同）；
4.  将 app 下载到本地：`heroku git:clone --app xxx`
5.  将 [huginn][4] 源代码全部下载拷贝到本地 app 的文件夹内；
6.  进入 app 文件目录，依次执行命令：`cp .env.example .env` 和 `bundle`；
7.  提交代码变更：`git add .`、`git commit -am 'commit code'`；
8.  最后，执行脚本：`bin/setup_heroku`，完成部署。

### 更新已经部署好的 Huginn

Huginn 的功能还在继续开发过程中，在将 Huginn 部署到 Heroku 平台之后，可以通过以下命令来更新 Huginn 的代码：

`git fetch origin  
git merge origin/master  
git push -f heroku master # 更新 Heroku 平台上的 Huginn 代码  
heroku run rake db:migrate # 迁移数据库到最新状态的 Huginn（尽管不需要每次更新时都运行该命令，但是安全起见，最好每次更新代码时，都运行该命令）`

### 使用自己的邮箱服务器

在安装过程中默认使用的是 SendGrid 的邮箱服务器（安装后需要自行配置），你也可以使用其他邮箱服务器，下面以谷歌邮箱服务器为例，需要进行以下配置：

`heroku config:set SMTP_DOMAIN=google.com  
heroku config:set SMTP_USER_NAME=you@gmail.com  
heroku config:set SMTP_PASSWORD=somepassword  
heroku config:set SMTP_SERVER=smtp.gmail.com  
heroku config:set EMAIL_FROM_ADDRESS=you@gmail.com # 指定显示的发件人邮箱地址`

### 备份你的数据

参见 [Heroku 的官方文档][5]

> 本文由 [Huginn 中文网][6] 翻译，已经获得项目作者授权，项目原文访问 [Deploy to Heroku][7]

 [1]: https://uptimerobot.com/
 [2]: https://www.heroku.com/
 [3]: https://toolbelt.heroku.com/
 [4]: https://github.com/cantino/huginn
 [5]: https://devcenter.heroku.com/articles/heroku-postgres-import-export
 [6]: http://huginn.cn
 [7]: https://github.com/cantino/huginn/blob/master/doc/heroku/install.md