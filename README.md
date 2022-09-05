

webServer
===============

## 简介

在Linux下用C++语言搭建web服务器，实现多用户请求服务器的图片等资源并及时响应。

* 使用 非阻塞Socket、Epoll (LT和ET)、线程池、事件处理 (Reactor和模拟Proactor) 的并发模型；
* 使用Epoll的IO多路复用技术提高并发量，并使用线程池避免频繁创建和销毁的开销；
* 通过单例模式实现异步日志系统，记录服务器运行状态、错误信息、访问数据的文件等；
* 状态机解析HTTP请求报文，通过GET和POST实现用户注册和登录、请求服务器图片和视频的功能。

## 快速运行

### 环境

* 服务器测试环境
  * Ubuntu 20.04.3 LTS
  * MySQL版本8.0.29
* 浏览器测试环境
  * Windows、Linux均可
  * Chrome
  * FireFox
  * 其他浏览器暂无测试

### 依赖库安装

* 测试前确认已安装MySQL数据库

  ```C++
  sudo apt-get install mysql-client
  sudo apt-get install mysql-server
  sudo apt-get install libmysql++-dev
  ```

  ```C++
  // 建立yourdb库
  create database yourdb;
  
  // 创建user表
  USE yourdb;
  CREATE TABLE user(
      username char(50) NULL,
      passwd char(50) NULL
  )ENGINE=InnoDB;
  
  // 添加数据
  INSERT INTO user(username, passwd) VALUES('name', 'passwd');
  ```

### 配置文件

* 修改`conf/TinyWebserverConfig.yml`中的服务器初始化信息

  ```YAML
   host: "0.0.0.0:9006" # 主机地址
   db:                  # 数据库信息
      url: localhost
      user: root
      password: root
      databasename: yourdb
      port: 3306
      maxconn: 15
   logs:                # 日志配置
      - name: root
         level: info
         appenders:
            - type: FileLogAppender
               foramt: '%d{%Y-%m-%d %H:%M:%S} %t %N %F [%p] [%c] %f:%l %m%n'
               file: /home/pudge/workspace/pudge-server/logs/root.txt
            - type: StdoutLogAppender
               foramt: '%d{%Y-%m-%d %H:%M:%S} %t %N %F [%p] [%c] %f:%l %m%n'
               
      - name: system
         level: info
         appenders:
            - type: FileLogAppender
               foramt: '%d{%Y-%m-%d %H:%M:%S} %t %N %F [%p] [%c] %f:%l %m%n'
               file: /home/pudge/workspace/pudge-server/logs/system.txt
            - type: StdoutLogAppender
               foramt: '%d{%Y-%m-%d %H:%M:%S} %t %N %F [%p] [%c] %f:%l %m%n'
  ```

### 编译运行

* build

  ```C++
  sh ./build.sh
  ```

* 启动server

  ```C++
  ./server
  ```

* 浏览器端

  ```C++
  ip:9006
  ```
# webServer
