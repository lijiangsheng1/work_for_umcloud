(Ubuntu 14.04)

## Install gitlab

### 安装步骤

安装gitlab需要的环境

	sudo apt-get install curl openssh-server ca-certificates postfix

gitlab依赖gitweb，同时依赖git版本>=1.7.10

	sudo apt-get install -y git git-core gitweb git-review

添加gitlab源，并安装gitlab

```
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
sudo apt-get install gitlab-ce
```

配置并启动gitlab

	sudo gitlab-ctl reconfigure

在浏览器中访问gitlab

默认密码为

```
Username: root
Password: 5iveL!fe
```

gitlab的配置文件为

	/etc/gitlab/gitlab.rb

修改该文件后，需要重新配置gitlab

	sudo gitlab-ctl reconfigure

默认使用postgresql & unicorn & nginx。配置文件位置为

```
/var/opt/gitlab/gitlab-rails/etc/unicorn.rb
/var/opt/gitlab/nginx/
```

### 配置gerrit帐号 & 测试帐号

使用root登录，添加用户gerrit，并设置密码，邮箱和公钥（公钥由gerrit帐号生成）。
添加用户demo，作为测试帐号，添加公钥
创建test-project，添加gerrit和demo为member，至少保证demo有review权限(reporter)，gerrit有push权限(devoloper)。

### 错误排查

1. 502 error

```
502
GitLab is taking too much time to respond
```

gitlab需要占用8080端口，请检查该端口是否被其他进程占用

## Gerrit

### install

创建用户gerrit2，将gerrit安装在该用户下

```
$ adduser gerrit2
$ su gerrit2
```

gerrit默认的数据库使用h2，我们替换成mysql。先建库

```
  CREATE USER 'gerrit2'@'localhost' IDENTIFIED BY 'secret';
  CREATE DATABASE reviewdb;
  GRANT ALL ON reviewdb.* TO 'gerrit2'@'localhost';
  FLUSH PRIVILEGES;
```

下载gerrit.war，并安装

```
mkdir gerrit
java -jar gerrit-2.12.war init -d /home/gerrit2/gerrit
```

期间会有很多问题要回答。需要注意的有

数据库使用mysql，plugins都要安装。

认证方式可选**development_become_any_account**，这样直接注册即可。这个后期可改。

其他默认即可。

gerrit默认第一个登录的用户为administrator.

默认gerrit使用tomcat启动

### gitlab支持

gerrit注册测试帐号demo，添加邮箱和公钥，所有信息和gitlab中帐号保持一致。

克隆test-project，

```
$ cd /home/gerrit2/gerrit/git
$ rm test-project.git -fr
$ git clone --bare git@123.59.92.252:demo/test-project.git
```
添加复制功能


```
$ vim etc/replication.config
[remote "test-project"]
  url = git@123.59.92.252:demo/${name}.git
  push = +refs/heads/*:refs/heads/*
  push = +refs/tags/*:refs/tags/*
  threads = 1
```
设置gerrit2用户的ssh config

```
$ cat .ssh/config
Host 123.59.92.252
   HostName 123.59.92.252
   IdentityFile ~/.ssh/id_rsa
```

添加到gitlab的信任关系

	sh -c "ssh-keyscan -t rsa 123.59.92.252 >> /home/gerrit/.ssh/known_hosts"
 	sh -c "ssh-keygen -H -f /home/gerrit2/.ssh/known_hosts"

重启gerrit服务。

### 配置和jenkins整合

gerrit注册帐号jenkins，添加邮箱和公钥（公钥由jenkins用户生成）。

使用管理员登录gerrit，

```
Projects->List->All-Projects
Projects->Access
Global Capabilities->Stream Events 点击 Non-Interactive Users
```

添加 jenkins@umcloud.com 用户到 ‘Non-Interactive Users’ 组

gerrit添加‘label verified’。

```
# cd /tmp
# git init cfg; cd cfg
# git config user.name 'admin'	#此处为gerrit admin帐号的username
# git config user.email 'admin@thstack.com'	#此处为gerrit admin帐号的email
# git remote add origin ssh://tianshuang@123.59.92.252:29418/All-Projects
# git pull origin refs/meta/config
# vim project.config        # 在文件末尾添加下面 5 行
[label "Verified"]
    function = MaxWithBlock
    value = -1 Fails
    value =  0 No score
　 value = +1 Verified
# git commit -a -m 'Updated permissions'
# git push origin HEAD:refs/meta/config
```

verified功能设置

在 Projects 的 Access 栏里，针对 Reference: refs/heads/ 项添加 Verified 功能：

```
Projects -> List -> All-Projects
Projects -> Access -> Edit -> 找到 Reference: refs/heads/* 项
-> Add Permission -> Label Verified -> Group Name 里输入 Non-Interactive Users -> 回车 或者 点击Add 按钮 -> 在最下面点击 Save Changes 保存更改
```


### 插件管理

安装插件

gerrit的core-plugins都在gerrit.war中，解压该war包（unzip），将plugins/*.jar拷贝到$(gerrit_path)/plugins/即可。

或者也可以远程安装，先开启远程安装

```
$ vim etc/gerrit.config
[plugins]
        allowRemoteAdmin = true

$ ./bin/gerrit.sh restart
```

然后执行如下命令

	 ssh -p 29418 -i ~/.ssh/umcloud  tianshuang@123.59.92.252  gerrit plugin install  -n replication $(pwd)/replication.jar

### 错误排查

1. pre-receive hook declined

```
Failed replicate of refs/heads/master to git@123.59.92.252:demo/cloudtest.git, reason: pre-receive hook declined
```

当gerrit在当前项目中的角色是developers时，可能会遇到该情况。 gitlab默认不允许developer去push branch

以project owner 身份登录gitlab，选择'Projected Branches'， 勾选'Developers can push'，即可。

2. replication问题排查

```
ssh -p 29418 -i ~/.ssh/umcloud  tianshuang@123.59.92.252  replication start --all
```

## Jenkins

### Install

可以直接下载并启动war包，也可以使用apt-get安装，如下：

```
wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
```

jenkins默认使用8080端口，如果冲突，需要修改

```
$ vim /etc/default/jenkins
HTTP_PORT=7000

$ sudo service jenkins restart
```

登录，开启用户注册功能，点击 -> 系统管理 -> Configure Global Security -> 勾上启用安全。

注册一个管理员帐号。登录后，关闭用户注册。

### 插件

安装jenkins插件，点击 系统管理 -> 管理插件。安装如下插件

	Gerrit Trigger, SSH plugin, Git plugin, LDAP plugin, URLTrigger Plug-in

设置 Gerrit Trigger


### 错误排查

1. 误删除管理员的所有权限

stop jenkins

编辑配置文件

```
$ vim /var/lib/jenkins/config.xml
#置为false
<useSecurity>false</useSecurity>
```

重启jenkins。再进行权限配置

也可以将如下配置写死到config.xml中

```
<authorizationStrategy class=”hudson.security.ProjectMatrixAuthorizationStrategy”>

<permission>hudson.model.Computer.Configure:USERNAME</permission>
<permission>hudson.model.Computer.Connect:USERNAME</permission>
<permission>hudson.model.Computer.Create:USERNAME</permission>
<permission>hudson.model.Computer.Delete:USERNAME</permission>
<permission>hudson.model.Computer.Disconnect:USERNAME</permission>
<permission>hudson.model.Hudson.Administer:USERNAME</permission>
<permission>hudson.model.Hudson.Read:USERNAME</permission>
<permission>hudson.model.Hudson.RunScripts:USERNAME</permission>
<permission>hudson.model.Item.Build:USERNAME</permission>
<permission>hudson.model.Item.Configure:USERNAME</permission>
<permission>hudson.model.Item.Create:USERNAME</permission>
<permission>hudson.model.Item.Delete:USERNAME</permission>
<permission>hudson.model.Item.Read:USERNAME</permission>
<permission>hudson.model.Item.Workspace:USERNAME</permission>
<permission>hudson.model.Run.Delete:USERNAME</permission>
<permission>hudson.model.Run.Update:USERNAME</permission>
<permission>hudson.model.View.Configure:USERNAME</permission>
<permission>hudson.model.View.Create:USERNAME</permission>
<permission>hudson.model.View.Delete:USERNAME</permission>
<permission>hudson.scm.SCM.Tag:USERNAME</permission>

</authorizationStrategy>
```

## 运维

### 修改访问url

1.gitlab相关
（以下gerrit, jenkins借用了gitlab的nginx作为反向代理）

```
$ cd /var/opt/gitlab/
$ vim nginx/conf/ci-cd-service.conf
$ vim nginx/conf/gitlab-http.conf
```
备份nginx/conf，修改gitlab配置

```
$ vim /etc/gitlab/gitlab.rb
external_url 'http://code.umcloud.io'
```

重新配置gitlab

注意reconfigure之前务必先备份nginx/conf，否则gitlab-ctl会覆盖nginx/conf/nginx.conf的配置

	$ sudo gitlab-ctl reconfigure

单独重启nginx的命令如下

	$ sudo gitlab-ctl restart nginx

2.gerrit相关

```
$ su - gerrit2
gerrit$ pwd
/home/gerrit2/gerrit
gerrit$ whoami
gerrit2
gerrit$ vim etc/gerrit.config
canonicalWebUrl = http://review.umcloud.io/
gerrit$ vim etc/replication.config
url = git@code.umcloud.io:${name}.git
gerrit$ ./bin/gerrit.sh restart
```

注意添加新url到ssh的known_hosts

```
sh -c "ssh-keyscan -t rsa code.umcloud.io >> /home/gerrit2/.ssh/known_hosts"
 sh -c "ssh-keygen -H -f /home/gerrit2/.ssh/known_hosts"
```

3.jenkins相关

```
$ su - jenkins
$ whoami
jenkins
$ pwd
/var/lib/jenkins
$ vim gerrit-trigger.xml
<gerritHostName>review.umcloud.io</gerritHostName>
<gerritFrontEndUrl>http://review.umcloud.io/</gerritFrontEndUrl>
$ vim hudson.tasks.Mailer.xml
<hudsonUrl>http://ci.umcloud.io</hudsonUrl>
```

以上gerrit-trigger.xml也可以登录jenkins，进入管理插件，点击gerrit trigger修改。

还需要修改各个项目的gerrit url

最后添加信任关系到ssh的known_hosts

	ssh jenkins@review.umcloud.io -p 29418

4.代码相关

修改各project的.gitreview文件中的url，修改git remote branch，包括origin和gerrit，重新review，测试流程是否跑通。


参考文档

http://longgeek.com/category/ci/

http://www.cnblogs.com/zhanchenjin/p/5032218.html
