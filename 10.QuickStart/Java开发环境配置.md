### Java开发环境配置

#### Linux For Java

##### Mac终端访问开发机

```shell 
# 登录relay跳板机（邮箱密码+如流认证）
ssh chenzhehao@relay.baidu-int.com
# 在relay登录开发机器（输入root密码）
ssh root@chenzhehao.bcc-bdbl.baidu.com
# 进入环境目录
cd /home/work/search
# 查看主机名
hostname
```

##### samba安装与使用

开发机安装samba：

```shell
# 使用yum安装samba
yum -y install samba samba-client samba-swat
# 查询是否安装成功
rpm -qa | grep samba
# 启动samba
/etc/init.d/smb start
# 修改配置文件（详见下文的samba配置）
vi /etc/samba/smb.conf
# 添加访问开发机用户（输入两次设置用户登录密码）
smbpasswd -a root
# 重启samba
/etc/init.d/smb restart
```

samba配置：粘贴下文配置到 `[global]` 标签下

```shell
[global]
diplay charset = utf8
unix charset = gbk
dos charset = gbk
workgroup = odp_map
netbios name = odp_map
server string = uc
security = user

# 本地机配置
[chenzhehao] # 本地机用户名
comment = uc
path=/home/users/chenzhehao # 本地用户目录
create mask = 0664
directory mask = 0775
writable = yes
valid users = chenzhehao # 用户登录名称（本地机用户名）
browseable = yes
```

本地机连接samba

```shell
# 本地机终端打开samba连接（输入samba账号密码）
open smb://chenzhehao.bcc-bdbl.baidu.com
```

##### Linux JDK安装与配置

JDK安装：

```shell
# 新建目录
mkdir -p /home/work/search
# 解压安装JDK到指定文件夹
tar -zxvf jdk-8u291-linux-x64.tar.gz -C /home/work/search
```

Java环境变量配置：

```shell
# 配置环境变量（参照下文Set Java Environment内容）
vi /etc/profile
# Set Java Environment
export JAVA_HOME=/home/work/search/jdk1.8.0_291
export JRE_HOME=$JAVA_HOME/jre
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
export CLASSPATH=$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:.
# 配置生效
source /etc/profile
# 查看Java版本
java -version
```

##### Linux Maven安装与配置

Maven安装：

```shell
# 新建目录
mkdir -p /home/work/search
# 解压安装maven到指定文件夹
tar -zxvf apache-maven-3.8.1-bin.tar.gz -C /home/work/search
```

Maven环境变量配置：

```shell
# 配置环境变量（参照下文Set Maven Environment内容）
vi /etc/profile
# Set Maven Environment
export MAVEN_HOME=/home/work/search/apache-maven-3.8.1
export PATH=$MAVEN_HOME/bin:$PATH
# 配置生效
source /etc/profile
# 查看maven版本
mvn -v
```

Maven配置本地库和阿里云私服：`.../maven/conf/settings.xml` 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.2.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.2.0 http://maven.apache.org/xsd/settings-1.2.0.xsd">
  <!-- 本地仓库 -->
  <localRepository>/Users/chenzhehao/Env/m2/repository</localRepository>
  <!-- 私服 -->
  <mirrors>
    <mirror>  
      <id>alimaven</id>  
      <name>aliyun maven</name>  
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>          
    </mirror>
  </mirrors>
</settings>
```

##### Linux Git安装与配置

Git安装：

```shell
# 新建目录
mkdir -p /home/work/search
# 解压安装JDK到指定文件夹
tar -zxvf jdk-8u291-linux-x64.tar.gz -C /home/work/search
```

Git用户配置：

```shell
# git用户名
git config --global user.name chenzhehao
# git用户邮箱
git config --global user.email chenzhehao@baidu.com
```

SSH密钥配置：生成密钥，复制密钥到git远程代码管理平台上

``` shell
# 生成密钥
ssh-keygen -t rsa
# 复制密钥到粘贴板
cat ~/.ssh/id_rsa.pub | clip    # Windows                                     
cat ~/.ssh/id_rsa.pub | pbcopy  # MacOS
cat ~/.ssh/id_rsa.pub # Linux手动复制下
```



#### MacOS For Java

#####  homebrew安装

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

##### Mac Git安装与配置

Git安装：

```shell
brew install git
```

Git配置：

>  参考上文Linux Git配置。

##### Mac Java配置

Java环境变量配置

```shell
# 配置环境变量（参照下文Set Maven Environment内容）
vi ~/.bash_profile
# Set Java Environment
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH:.
export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.
# 查看java版本
java -version
```

Java卸载：

```shell
sudo rm -fr /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin
sudo rm -fr /Library/PreferencesPanes/JavaControlPanel.prefPane
sudo rm -fr ~/Library/Application\ Support/Oracle/Java
```

##### Mac Maven配置

Maven环境变量配置

```shell
# 配置环境变量（参照下文Set Maven Environment内容）
vi ~/.bash_profile
# Set Maven Environment
export MAVEN_HOME=/Users/chenzhehao/Env/apache-maven-3.8.1
export PATH=$MAVEN_HOME/bin:$PATH
# 查看maven版本
mvn -v
```

Maven配置本地库和阿里云私服：`.../maven/conf/settings.xml` ‘

> 参考上文Linux Maven配置



#### Windows

##### 安装

- Browser：Chrome。
- Base：jdk、jre、nodejs。
- Env：maven。
- IDE：IDEA、DataGrip、Navicat。
- Editor：VSCode、typora。
- Tools：7-Zip、Git、Xshell、Postman、RedisDesktopManager。
- setting：maven_setting.xml、bookmarks.html。

##### 配置

- 配置环境变量：jdk、maven。
- Maven配置：maven_setting.xml。
- 破解：JetBrain、Navicat、Xshell。
- idea配置：插件、代码格式。
- Chrome：导入书签bookmarks.html。

