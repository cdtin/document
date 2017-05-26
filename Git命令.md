### 什么是GIT
- 是一个源代码管理工具
- 在一个项目中，凡是由开发人员编写的都算是源代码 
- GIT是Linux之父为了管理Liunx源代码写的一个工具
- https://guides.github.com

### 安装GIT 
- Widows下载安装：https://git-scm.com/
- Linux和Mac自带Git，不需要安装

### GIT命令操作
- 初始化一个本地GIT仓储
```shell
cd 到当前目录
git init  //初始化一个本地的仓库
```
> 就是在本地文件夹中添加了一个.git的文件夹用于记录所有的项目变更信息

- 查看本地仓储的变更状态
```
//-s输入简要的变更日志，前面A表示是add过得，M表示修改过得文件  
//??表示未add
git status -s 
```
> 第一次查看，显示的是一坨没有被跟踪的文件

- 将一个没有被跟踪文件添加到跟踪列表
```shell
//.表示当前目录所有的文件(也可以使用 --all)，指定文件写文件名夹扩展名
git add . 
```

- 添加本地GIT忽略清单
```
在代码库文件夹的根目录添加一个.gitignore文件
```
> 在代码库文件夹中的跟目录添加一个.gitignore文件,windows下命令：type nul > [文件名]  
> 此文件用于说明忽略的文件有那些  
> 直接将要忽略的文件写入文件即可，一个文件一行

- 提交被托管的文件变化到本地仓储
```
//将本地的变化提交到本地的仓储中
git commit -m '写提交日志'
```

- 对比差异
```
git diff  //用于对比版本差异
```

- 查看日志
```
git log
```

- 代码还原某个版本
```
git reset --hard [log文件中commit哈希值前六位]
```

- 删除git中的文件

```
git rm . -r  //.表示文件夹下的所有文件，-r 允许删除的意思
```

### GITHUB基本操作
> 在push或pull需要是用GIT添加本地版本仓储

- 链接远程
```
    git remote add origin [github仓储链接]
```
- push
```
git push -u origin master
```
- pull
```
git pull origin master
```

- 分支
    > 分支就是将原有分支在复制复制一份形成一个新的分支，  
    > 在修改的时候不会影响彼此  

    + 查看当前有几个分支
    
    ```
        git branch
    ```

    + 创建一个新的分支
    
    ```
        git branch [分支名称]
    ```

    + 变更分支
    
    ```
        git checkout [分支名称]
    ```

    + 克隆分支
    
    ```
    git clone [分支的url] . //.表示当前目录
    ```




