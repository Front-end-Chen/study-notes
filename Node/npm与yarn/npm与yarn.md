# npm与yarn

## 1.包与npm包管理器

1.什么是包？

  我们电脑上的文件夹，包含了某些特定的文件，符合了某些特定的结构，就是一个包。

2.node的包由包结构和包描述文件两个部分组成。

​    1)包结构：用于组织包中的各种文件

​    2)包描述文件：描述包的相关信息，以供外部读取分析

3.一个标准的包, 应该包含哪些内容？

​    1) package.json ------- 描述文件（包的 “说明书”，必须要有！）

​    2) bin ---------------- 可执行二进制文件

​    3) lib ---------------- 经过编译后的js代码

​    4) doc ---------------- 文档（说明文档、bug修复文档、版本变更记录文档）

​    5) test --------------- 一些测试报告

4.如何让一个普通文件夹变成一个包？

- 让这个文件夹拥有一个: package.json文件即可, 且package.json里面的内容要合法。

- 执行命令: 
	
	npm init    //手动创建包与package.json
	
	npm init -y    //自动创建包与package.json

- 包名的要求: 不能有中文、不能有大写字母、同时尽量不要以数字开头、不能与npm仓库上其他包同名。

5.npm 与 node的关系？（npm: node package manager）

- 安装node后自动安装npm（npm是node官方出的包管理器, 专门用于管理包）

## 2.npm常用命令（重点！）

### 2.1【搜索】：

​    1.npm search xxx（了解！）

​    2.通过npm官网搜索：www.npmjs.com

### 2.2【安装】：(安装之前必须保证文件夹内有package.json，且里面的内容格式合法)

​    1.npm install xxx --save   或   npm i xxx -S   或   npm i xxx（重点！）

- 注：

​        (1).局部安装完的第三方包，放在当前目录中node_modules这个文件夹里

​        (2).安装完毕会自动产生一个package-lock.json(npm版本在5以后才有)，里面缓存的是每个下载过的包的地址，目的是下次安装时速度快一些。

​        (3).当安装完一个包，该包的名字会自动写入到package.json中的【dependencies(生产依赖)】里。npm5及之前版本要加上--save后缀才可以。

​    2.npm install xxx --save-dev  或  npm i xxx -D （重点！）: 安装xxx包，并将该包写入到【devDependencies(开发依赖中)】

- 注：什么是生产依赖与开发依赖？

​      1）只在开发时(写代码时)时才用到的库，就是开发依赖 ----- 例如：语法检查、压缩代码、扩展css前缀的包。

​      2）在生产环境中(项目上线)不可缺少的，就是生产依赖 ------ 例如：jquery、bootStrap等等。

​      3）注意：某些包即属于开发依赖，又属于生产依赖 -------例如：jquery。

​    3.npm i xxx -g （重点！）: 全局安装xxx包（一般来说，带有指令集的包要进行全局安装，例如：browserify、babel等）

- 全局安装的包，其指令到处可用，如果该包不带有指令，就无需全局安装。

- 查看全局安装的位置：npm root -g

​    4.npm i xxx@yyy（重点！）: 安装xxx包的yyy版本

​    5.npm i （重点！）: 安装package.json中声明的所有包。项目经理上线时会删package.json的开发依赖。

### 2.3【移除】：

- npm remove xxx （重点！）: 在node_module中删除xxx包，同时会删除该包在package.json中的声明

- npm uninstall -g 包名 : 删除全局安装的npm包

### 2.4【其他命令】：

​    1.npm aduit fix（了解！） : 检测项目依赖中的一些问题，并且尝试着修复。

​    2.npm view xxx versions （重点！）: 查看远程npm仓库中xxx包的所有版本信息

​    3.npm view xxx version : 查看npm仓库中xxx包的最新版本

​    4.npm ls xxx (-g)（重点！）: 查看我们所(全局)安装的xxx包的版本

### 2.5【关于版本号的说明】（了解！）：

- "^3.x.x" ：锁定大版本，以后安装包的时候，保证包是3.x.x版本，x默认取最新的。

- "~3.1.x" ：锁定小版本，以后安装包的时候，保证包是3.1.x版本，x默认取最新的。

- "3.1.1" ：锁定完整版本，以后安装包的时候，保证包必须是3.1.1版本。

### 2.6 替换npm仓库地址为淘宝镜像地址，不需要直接安装cnpm

- npm config set registry https://registry.npm.taobao.org/

## 3.yarn的基本使用（重点！）

1.yarn的使用和npm差不多, 但yarn比npm更好用！

npm install yarn -g: 全局安装yarn

**特别注意！**

- 由于yarn的全局安装位置与npm不同，所以要配置yarn的全局安装路径到环境变量中，否则全局安装的包不起作用。

- 具体操作如下：

​    1） 安装yarn后分别执行 yarn global dir 命令，yarn global bin 命令。

​    2） 将上述两步返回的路径配置到电脑环境变量中即可。

2.yarn命令与npm命令的对应关系如下：

- 初始化项目: 

	yarn init -y

	npm init -y

- 下载项目的所有声明的依赖: 

	yarn

	npm install

- 下载指定的运行时依赖包: 

	yarn add xxxx@3.2.1

	npm install xxxxx@3.2.1 -S

- 下载指定的开发时依赖: 

	yarn add xxxxx@3.2.1 -D

	npm install xxxxx@3.2.1 -D

- 全局下载指定包: 

	yarn global add xxx

	npm install xxx -g

- 删除依赖包:

	yarn remove xxx

	yarn global remove xxx

	npm remove xxx

	npm uninstall -g xxx

- 查看某个包的信息:

	yarn info xxx

	npm info xxx

- 设置淘宝镜像:

	yarn config set registry https://registry.npm.taobao.org

	npm config set registry https://registry.npm.taobao.org

