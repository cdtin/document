# 混合式App环境搭建

## 系统环境搭建

> `Android`：Java JDK、C++环境、Android ADT、 Node环境、Git环境  
> `IOS`：XCode (需要有mac电脑)、Node Git

### 项目依赖

- > `Cordova`(打包工具): npm install –g cordova

- > `Ionic`(框架): npm install –g ionic

### Git

- > 安装完git后，在环境变量--用户变量新建变量:

    ```
    变量名:GIT_HOME
    变量值:[Git安装路径]\bin;
    ```

    ```
    在path中新建设置：%GIT_HOME%
    ```

> 配置完成后再CMD中输入`git`，没有提示：不是内部或外部命令，那么就设置成功了

### Android

- > 将android sdk安装并下载完成后，在环境变量中新建变量：  

    ```
    变量名：ANDROID_HOME
    变量值：[android sdk安装路径]
    ```
- > 在path中新建设置：   

    ```
    %ANDROID_HOME%\platform-tools
    %ANDROID_HOME%\tools
    ```
> 配置完成后再CMD中输入adb，没有提示：不是内部或外部命令，那么就设置成功了

`sdk下载一直是个头疼的问题，不会翻墙的朋友很无耐，前不久发现了一个国内站，提供Android这块的工具下载，非常齐，并有教程。
[http://www.androiddevtools.cn/](http://www.androiddevtools.cn/)`

### JAVA

- > 下载并安装java jdk后，需要配置环境变量  

    ```
    变量名：JAVA_HOME
    变量值：[jdk安装路径]
    ```
    ```   
    变量名：CLASSPATH
    变量值：.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar; 
    记得前面有个"."。
    ```
    ```
    变量名：Path
    变量值：%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
    ```
> 配置完成后在CMD中依次验证命令：java -version、java、javac 不报 不是外部或内部命令 就说明设置成功了。

## 项目搭建 
 
### 创建项目模板

```
ionic提供三套默认模板（自动会到github上下载），在cmd中输入：
ionic start [下载到本地后模板名称] [blank | tabs | sidemenu ]
blank: 一套空模板
tabs：包含tab选项卡
sidemenu:包含菜单
```

#### 模拟器运行 (不常用，有点卡)

- > 在CMD中输入：

    ```
    Android：ionic emulate android
    IOS: ionic emulate ios
    ```

#### 打包APP

- > 1．添加项目平台

    ```
    Android：ionic platform add android
    IOS: ionic platform add ios
    ```
- > 2．打包

    ```
    Android: ionic build android
    IOS: ionic build ios
    ```

- > 3．直接运行在手机

    ```
    需要开启android调试功能
    ionic run android
    ```

