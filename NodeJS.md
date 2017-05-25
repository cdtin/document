# 配置Node环境步骤

> 适用于Windows，Linux下配置非常简单，几条命令行就搞定，后面有空了可以做个文档

- 1.<a href='#nvm'>NVM</a> -- 用于NodeJS版本切换
- 2.NRM -- 切换NPM源
- 3.NPM -- NodeJS包管理工具(还有bower,n他们的使用方式都差不多，就不介绍了，毕竟NPM是主流)

<i id='nvm'></i>
## 什么是NVM
NVM(Node Version Manager)是Node.js的一个版本管理工具，可以切换不同版本的Node.js，非常方便。

### 安装NVM
- 下载并安装在全英文路径下：[下载NVM](https://github.com/coreybutler/nvm-windows/releases)
 
2.选择安装路径（全英文路径）
 
3．选择切换node的路径（也要全英文）
  
安装成功后，打开cmd，输入：nvm –v查看nvm版本，如果显示一下信息说明安装成功。
 
安装Node.js
	在nvm安装安装目录下安装shift+鼠标右键，在弹出的框中选“在此次打开命令行窗口”；
输入命令: nvm inatall 5.5.0 ，后面的5.5.0是安装nodejs的版本号。
安装成功后，提示一下信息：
 
输入命令： nvm ls ，可以查看当前安装的node.js的版本。
 
安装文件下也多出了安装的nodejs版本文件夹。
 
启动Node.js
使用命令启动或切换nodejs版本：nvm use 5.5.0 ，后面的数字是启动nodejs的版本号（启动的nodejs版本必须是安装成功的nodejs版本），启动成功后，显示如下信息：
 
然后可以通过 node –v ，查看当前启动的nodejs的版本
 

统一管理npm包
在切换nodejs版本的时候npm包是无法切换的，这时我们可以通过配置环境变量的方式统一管理npm包，这样在切换不同版本的时候也可以统一调用同一个npm包。
因为程序在运行的时候是先找程序本身下的依赖环境，如果找不到就会在环境变量下配置的路径中去寻找。
1.	在nvm目录中新建一个文件夹命名为 npm
 
系统默认npm包下载的路径是在：
C:\Users\LiouTin\AppData\Roaming，可以通过命令：
npm config get prefix，查看当前系统npm路径，
下面有两个npm和npm-cache文件夹，npm就是存储下载的包，我可以通过修改保存路径来实现统一管理，方法如下：(在cmd中输入一下命名)
npm路径：npm config set prefix "D:\Develop\nvm\npm" 
npm-cache路径：npm config set cache "D:\Develop\nvm\npm-cache"

2.配置npm环境变量：
	新建一个用户变量:
	变量名：NPM_HOME
	变量值：D:\Develop\nvm\npm（自己安装的路径）
保存后在设置用户path路径：
;%NPM_HOME%
保存并退出。
	在命令行可以通过set命令查看是否设置成功
 
安装npm包
	输入命令：npm install bower –g ，bower是包名称，-g表示全局安装的意思（global），安装成功后可以在配置的npm路径下查看到下载的包（模块）
	 

安装gulp自动化工具
首先安装全局安装gulp，安装后将在配置的npm主目录中，输入命令：npm install --global gulp-cli。
安装完成后，将路径cd到工程根目录下，运行命令：npm install -–save-dev gulp，根目录下将出现node_modules这样的文件夹，里面将有大量的依赖文件。
注意：
每次在使用一个新的路径的时候，如果要用npm命令安装包，首先需要cd到当前目录，在使用命令： npm init ,初始化目录，一路回车，并将在工程根目录下生成一个package.json这样一个文件。在通过npm install [包名] –save,将生成一个node_modules的文件夹，里面就是下载的包。
安装完成后，新建一个gulpfile.js的文件。
这个是个固定文件名，名称必须一样 ，这样就可以在gulpfile.js文件中编写相关自动化的脚本

gulp常用安装包
安装须知：
	安装前需要cd到工程目录下，在通过npm install package-name –save-dev命令安装
	安装依赖问题：
		--save-dev:开发依赖，项目后期不用就不用项目依赖
		--save:项目依赖，项目后期部署如果要用就项目依赖
执行命令：gulp name(名称)执行

编译 Less：gulp-less
https://www.npmjs.com/package/gulp-less
创建本地服务器：gulp-connect
https://www.npmjs.com/package/gulp-connect
浏览器同步测试工具browser-sync
https://browsersync.io/
合并文件：gulp-concat
https://www.npmjs.com/package/gulp-concat
最小化 js 文件：gulp-uglify
https://www.npmjs.com/package/gulp-uglify
重命名文件：gulp-rename
https://www.npmjs.com/package/gulp-rename
最小化 css 文件：gulp-minify-css
https://www.npmjs.com/package/gulp-mini
压缩html文件gulp-minify-html
https://www.npmjs.com/package/gulp-minify-l
最小化图像：gulp-imagemin
https://www.npmjs.com/package/gulp-imagemin
