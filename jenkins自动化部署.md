\#基于gitlab&jenkins的自动化部署实践





环境：jenkins地址: http://10.50.2.70:8080/admin/admin

入门

1. 新建任务

![image-20210918140820590](/Users/miao/Library/Application Support/typora-user-images/image-20210918140820590.png)



2. 输入任务名称

![image-20210918140954620](/Users/miao/Library/Application Support/typora-user-images/image-20210918140954620.png)





3. 配置

   ![image-20210918141123351](/Users/miao/Library/Application Support/typora-user-images/image-20210918141123351.png)



配置1

![image-20210918141321110](/Users/miao/Library/Application Support/typora-user-images/image-20210918141321110.png)



配置权限

![image-20210918141444024](/Users/miao/Library/Application Support/typora-user-images/image-20210918141444024.png)



添加后的效果

![image-20210918141602426](/Users/miao/Library/Application Support/typora-user-images/image-20210918141602426.png)



指定分支

![image-20210918141634321](/Users/miao/Library/Application Support/typora-user-images/image-20210918141634321.png)



构建 触发器



Build when a change is pushed to GitLab. GitLab webhook URL: http://10.50.2.70:8080/project/hz-data-exchange-business

![image-20210918141707604](/Users/miao/Library/Application Support/typora-user-images/image-20210918141707604.png)





构建环境

![image-20210918141746989](/Users/miao/Library/Application Support/typora-user-images/image-20210918141746989.png)





构建后的操作

![image-20210918142012140](/Users/miao/Library/Application Support/typora-user-images/image-20210918142012140.png)



![image-20210918141919969](/Users/miao/Library/Application Support/typora-user-images/image-20210918141919969.png)

构建-执行shell

![image-20210918152355518](/Users/miao/Library/Application Support/typora-user-images/image-20210918152355518.png)



构建-构建后的操作

![image-20210918152431235](/Users/miao/Library/Application Support/typora-user-images/image-20210918152431235.png)













![image-20210918143244621](/Users/miao/Library/Application Support/typora-user-images/image-20210918143244621.png)





![image-20210918143435385](/Users/miao/Library/Application Support/typora-user-images/image-20210918143435385.png)





构建

![image-20210918153324937](/Users/miao/Library/Application Support/typora-user-images/image-20210918153324937.png)



构建 -服务器

![image-20210918153350354](/Users/miao/Library/Application Support/typora-user-images/image-20210918153350354.png)





构建后操作

![image-20210918153226797](/Users/miao/Library/Application Support/typora-user-images/image-20210918153226797.png)





gitlab 配置 we bhook

![image-20210918142953035](/Users/miao/Library/Application Support/typora-user-images/image-20210918142953035.png)



![image-20210918143003526](/Users/miao/Library/Application Support/typora-user-images/image-20210918143003526.png)

配置gitlab--webhook



![image-20210918153832906](/Users/miao/Library/Application Support/typora-user-images/image-20210918153832906.png)



error

1. 

![image-20210918154918842](/Users/miao/Library/Application Support/typora-user-images/image-20210918154918842.png)

处理：关闭不需要填写的模块



![image-20210918155323374](/Users/miao/Library/Application Support/typora-user-images/image-20210918155323374.png)



jenkins的配置

jenkins -> 系统配置
jenkins location, jenkins url: 自己的服务ip地址 gitlab gitlab host url: https://gitlab.com/ Credentials: gitlab Api token / api token

自己工程的配置页:
源码管理: repository url: 项目的git地址 Credentials: Gitlab API token -> API token || 账号/密码 指定分支

构建触发器: Build when a change is pushed to GitLab. GitLab webhook URL: http://121.41.104.96/project/lanui

Secret token

构建环境: Provide Node & npm bin/ folder to PAT

构建

执行shell

cd /root/.jenkins/workspace/lanui // 服务器jenkins工作目录
npm install
npm run build
cd dist
rm -rf lanui.tar.gz
tar -zcvf lanui.tar.gz
cp lanui.tar.gz /usr/local/nginx
cd /usr/local/nginx
tar -xzvf lanui.tar.gz
构建后操作 send build articatcs over SSH

Name: ip地址 Source files: dist/** 当前目录/dist/所有文件 Remote directory: nginx文件目录

插件管理 NodeJS Plugin Gitlab Hook Plugin Gitlab Hook Plugin Generic Webhook Trigger Publish Over SSH 通过SSH连接其他Linux机器，远程传输文件及执行Shell命令
gitlab 的配置

settings -> intergrations Active interations, jenkins

Jenkins server URL: jenkins 构建触发器的GitLab webhook URL

settings -> webhooks URL: jenkins 构建触发器的GitLab webhook URL Secret token

user settings Access Tokens Select scopes 只勾选api