---
title: Vue实践 【一】项目初始化
date: 2018-01-13 17:01:17
tags: vue, npm, node, cnpm, vue-cli
---

### 一 环境搭建
 1. 本地安装node和npm，地址和安装方法可以官网 （[官网链接][1]）。官网下载较慢的话，windows安装版本可以通过[我的百度云][2]，密码sq6a，版本是v8.9.4。后续版本升级的话可以通过如下代码：
 ```
 npm install -g n
 n stable # 安装最新的版本

 ```
 2. 【可选】npm安装成功后，可以安装cnpm[淘宝镜像][3]，用cnpm替代npm，速度比npm快
 ```
 npm install -g cnpm --registry=https://registry.npm.taobao.org

 ```
 3. 全局安装vue-cli,vue项目脚手架，用于初始化vue项目
 ```
 npm install -g vue-cli
 cnpm install -g vue-cli

 ```
 4. 安装成功后，可通过相应的命令检查：
 ```
 npm -v # 检查npm版本
 node -v # 检查node版本
 vue -v # 检查vue-cli的版本，非vue版本
 cnpm -v

 ```
### 二 项目初始化
通过命令初始化一个空的项目，关于vue-cli的说明可见[Github地址][4]
```
vue init <template> <project-name>
```

至此，我们通过命令创建自己的项目。命令执行后会有一些列的选项可供选择，自己自行填写，也可全部按照默认的来。
```
vue init webpack todolist-vue
cd todolist-vue
npm install # 如果npm较慢，建议使用cnpm
```

### 三 项目运行
执行如下命令，运行项目，运行成功后，会自动打开浏览器，如果未打开，可以手动输入http://localhost:8081/#/ ，端口号可能不同，既可以看到vue页面
```
npm run dev # 命令可能不同，具体可以看目录下的README.md文件

```


  [1]: https://nodejs.org/zh-cn/
  [2]: https://pan.baidu.com/s/1o9KzGjo
  [3]: https://npm.taobao.org/
  [4]: https://github.com/vuejs/vue-cli
