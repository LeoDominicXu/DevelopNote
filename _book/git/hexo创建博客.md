---
title: 使用Hexo创建个人的博客
---
这里讲的是git+hexo+github创建个人博客，hexo是一个基于node的博客框架，当然如果另外两个东西不知道是啥的话也就没有必要继续下去了。

## 1、预备工作
1. 注册github账号
2. 安装git
3. 安装node

## 2、好啦接下来可以安装Hexo了
``` bash
$ npm install -g hexo
```
## 3、创建博客根目录
```bash
$ mkdir hexo
$ cd hexo
$ hexo init #该命令会将hexo所需文件自动下载到hexo文件夹下。
$ npm install #安装依赖包
#本地运行
$ hexo generate
$ hexo server
```
输入完以上命令打开浏览器输入网址localhost:4000查看，运行显示了相关页面说明成功。当前网站建立在本地而已。

## 4、配置ssh连接github（具体参考[配置ssh连接github](/2017/04/12/setSSH/)）
## 5、使用Hexo克隆主题
```bash
hexo clean
hexo g
hexo s
```
自己使用的是bluelake主题，比较喜欢，以这款主题为例。
git@github.com:chaooo/hexo-theme-BlueLake.git

克隆主题
```bash
git clone git@github.com:chaooo/hexo-theme-BlueLake.git themes/BlueLake
```
配置
修改hexo根目录下的 _config.yml ： theme: BlueLake

更新
cd themes/BlueLake
git pull

## 6、部署博客
部署Github前需要配置_config.yml文件
```bash
deploy:
  type: github
  repository: http://github.com/username/username.github.io.git
  branch: master
```
username为你的github用户名
注意：type:空格github。都要使用空格，自己遇到过这个问题，结果怎么都上传不上去，所以提醒下。

上传
```bash
hexo clean
hexo g
hexo d
```
会让你输入用户名和密码，依次输入就好。

本地查看
```bash
hexo g
hexo s
```
浏览器输入localhost:4000，查看主题是否成功。

## 7、给博客绑定域名
1. 项目添加域名
```bash
$ cd hexo/public
$ echo www.xxx.com > CNAME
# 重新发布
$ hexo g
$ hexo d
```
2. DNS设置
```bash
注册DNSPOD，添加域名
192.30.252.153
192.30.252.154
以上为github提供的ip
```
等待DNS刷新
可能需要等待一段时间。

## 8、写文章并发布
路径hexo\source_posts下新建文件就可以了 XXX.md
使用Markdown语法进行书写
简书Markdown语法指南
注意：文档最上面写

# Q&A

1、搭建 hexo，在执行 hexo deploy 后,出现 error deployer not found:github 的错误

npm install hexo-deployer-git --save 改了之后执行，然后再部署试试
