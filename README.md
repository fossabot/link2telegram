<h1 align="center">
    <img width="500" src="https://raw.githubusercontent.com/Crystal-Moling/link2telegram/master/Banner.png"/><br>
	Link2Telegram
</h1>

# 介绍
通过插件连接到Telegram Bot

# 加载
* 复制jar包到项目根目录
* ## maven
  * 添加以下内容到pom.xml
  ```
    <dependency>
      <groupId>org.crystal.link2telegram</groupId>
      <artifactId>link2telegram</artifactId>
      <version>1.0</version>
      <scope>system</scope>
      <systemPath>${project.basedir}/link2telegram-1.0.jar</systemPath>
    </dependency>
  ```
* ## Gradle
  * 添加以下内容到build.gradle
  ```
    dependencies {
      compile files('link2telegram-1.0.jar')
    }
  ```
* 为项目添加引用
  ```java
  import org.crystal.link2telegram.Link2telegram;
  ```
# 文档
[发送消息](#发送消息)  
[获取消息](#获取消息)  
[TPS监测](#TPS监测)  
[获取服务器基本数据](#获取服务器基本数据)  
[config.yml](#config)  
[Bot命令](#Bot命令)
## 发送消息
使用机器人发送消息
  ```java
  Link2telegram.L2tAPI().sendMsg("<待发送的消息>");
  ```
注意:该方法不会对将要发送的消息格式化处理，如果要进行格式化，请使用以下代码:
  ```java
  Link2telegram.L2tAPI().sendFormatedMsg("<待发送的消息>", "<消息种类>");
  ```
目前支持的消息种类有Status/Warn/Info,输入其他字符将被格式化为无前缀信息
## 获取消息
获取机器人收到的最近一条消息  
### 使用方法
* 在插件主类添加`implements Listener`  
* 在`onEnable`中注册监听器  
  ```java
  getServer().getPluginManager().registerEvents(this, this);
  ```
* 使用如下代码监听获取消息事件
  ```java
  @EventHandler(priority = EventPriority.MONITOR)
  private void GetUpdateListener(GetUpdateEvent event){
  }
  ```
* 使用`event.GetMessage()`获取接收到的信息  

返回值类型：String
## 获取命令
获取机器人获取的命令(以"/"开头)  
### 使用方法
* 在插件主类添加`implements Listener`
* 在`onEnable`中注册监听器
  ```java
  getServer().getPluginManager().registerEvents(this, this);
  ```
* 使用如下代码监听命令事件
  ```java
  @EventHandler(priority = EventPriority.MONITOR)
  private void GetCommandListener(OnCommandEvent event){
  }
  ```
* 使用`event.GetCommand()`获取接收到的命令文本(不带"/")  

`/status和/sudo为内置命令,无法被监听.详见`[Bot命令](#Bot命令)  
返回值类型：String[]
## TPS监测
该方法可以在服务器TPS超出或低于设定的阈值时发送警告消息  
设定的阈值可以在config.yml中修改
### 获取TPS
  ```java
  Link2telegram.L2tAPI().getServerTPS();
  ```
返回值类型：double[]
## 获取服务器基本数据
该方法可以返回服务器当前的CPU和内存使用情况
  ```java
  Link2telegram.L2tAPI().getServerStatus();
  ```
返回值类型：int[CPU占用,内存占用]  
该方法也可以用自带Bot命令`/status`获取状态  
详见[Bot命令](#Bot命令)
## config
```
# 从@BotFather处获取的BotToken
BotToken: 123456:qwertyuiopASDFGHJKL
# 将信息发送给该chatId
SendMsgToChatID: 1234567890
Proxy: 
  #代理服务器地址,不填则不使用代理
  Hostname: 127.0.0.1
  # 代理服务器端口
  Port: 7890
DefaultMsg:
  # 服务器启动时发送的消息
  PluginOnEnableMsg: '服务器启动'
  # 服务器关闭时发送的消息
  PluginOnDisableMsg: '服务器关闭'

# 插件功能
TPSMonitor:
  # 是否启用TPS监测
  Enabled: true
  # TPS监测间隔时长,单位为秒
  TPSCheckTimeout: 5
  # TPS最大阈值
  MaxTPSThreshold: 22
  # TPS过高的警告信息
  TPSTooHighInformation: 'TPS过高,当前TPS:'
  # 是否在警告信息后添加当前TPS
  THIEndedWithTPS: true
  # TPS最低阈值
  MinTPSThreshold: 18
  # TPS过低的警告信息
  TPSTooLowInformation: 'TPS过低,当前TPS:'
  # 是否在警告信息后加上当前TPS
  TLIEndedWithTPS: true
```
## Bot命令
此处列出插件默认自带命令,这些命令无法被`OnCommandEvent`监听
### /status
获取当前服务器基本信息,返回消息格式：
```
ℹ️[信息] 12 : 34 : 56
CPU:10%
Memory:45%
```
### /sudo
用于执行命令  
示例：如要执行命令`/say test`,则发送命令`/sudo say test`
# 依赖
[okhttp](https://github.com/square/okhttp)  
[java-telegram-bot-api](https://github.com/pengrad/java-telegram-bot-api)

# License
MIT