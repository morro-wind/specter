一. upgrade checklist

目标版本
https://www.atlassian.com/software/jira/update
确定自定义文件列表
- <jira-home-directory>/atlassian-jira/ directory
 - <jira-home-directory>/conf/server.xml
  - <jira-home-directory>/bin/setenv.sh
  - <jira-home-directory>/bin/user.sh
  - <jira-home-directory>/atlassian-jira/WEB-INF/classes/jira-application.properties
确定目标版本支持的平台
https://confluence.atlassian.com/adminjiraserver0822/supported-platforms-1142236965.html

二. 升级准备

停止Jira 服务
- <jira-home-directory>/bin/stop-jira.sh
备份数据库数据
pg_dump -h dbhost -U jira -d jira -W > jira.sql
备份data 目录
tar zcf <jira.home>.tgz <jira.home>
备份Jira 程序目录
tar zcf <jira-home-directory>.tgz <jira-home-directory>

三. 升级操作

下载目标版本程序
解压程序，替换原应用程序目录
修改自定义文件列表文件
- <jira-home-directory>/atlassian-jira/ directory
 - <jira-home-directory>/conf/server.xml
  - <jira-home-directory>/bin/setenv.sh
  - <jira-home-directory>/bin/user.sh
  - <jira-home-directory>/atlassian-jira/WEB-INF/classes/jira-application.properties
修改应用程序目录及数据目录权限
chown -R jira:jira <jira-home-directory> <jira.home>
启动Jira 应用程序
- <jira-home-directory>/bin/start-jira.sh
登录web 页面验证服务
升级插件
重建索引

四. 参考资料

升级手册页
https://confluence.atlassian.com/adminjiraserver/upgrading-jira-manual-938846939.html
agent
https://zhile.io/2018/12/20/atlassian-license-crack.html
机器码
antpool: B4RW-PAII-YBUA-ZZDG








