# uniapp新闻页面

### 介绍
uniapp练习小demo，这是我第一个通过代码仓库创建出来的小demo  
练习：请求数据，下拉刷新，浏览记录，git的使用。  
通过这个demo熟悉uniapp的常用api，以及git的一些基本操作。

### 我的开发环境
HBuiderx + 微信开发者工具 + 手机模拟器。

### 接口
#### 导航类目
https://ku.qingnian8.com/dataApi/news/navlist.php
#### 新闻大纲数据
https://ku.qingnian8.com/dataApi/news/newslist.php
#### 新闻详情
https://ku.qingnian8.com/dataApi/news/detail.php

## 下面是我的学习笔记

## 一、业务逻辑记录
### 1. 触底刷新数据
这个挺有意思，`onReachBottom`这个函数是uniapp支持的一个生命周期函数，当页面到底部时调用的。  
触底刷新第一是要请求下一页数据，并往数组中添加下一页的数据；第二是要给用户提示。
### 2. 加载数据时的loading提示
这个就是上面触底刷新数据所说的第二点：要给用户提示，增加用户体验。  
1.首先要在尾部加一个提示，只有在有数据的时候才显示  
```html
<view class="loading" v-if="newsArr.length">
	<view v-show="loading === 1">数据加载中......</view>
	<view v-show="loading === 2">没有更多数据了~~</view>
</view>
```
2.data中设置loading为0（0默认不显示 1表示加载中 2表示没有数据）  
3.当使用触底函数时，把loading变成1，然后发请求  
4.发请求返回结果做个判断，如果没数据了就把loading变成2  
```javascript
if(res.data.length === 0) {
	this.loading = 2;  //如果下拉请求不到数据了，就提示
}
```
5.当然，最好在触底函数中首行做个判断，如果没数据了就不发请求了  
```javascript
if(this.loading === 2) return;  //这行如果不写，每次触底都会提示加载并发请求，不太好
```
### 3.uniapp中的一些其他注意点
（1）全局组件不需要注册引入，只要在components目录中，就可以直接写标签使用  
（2）发请求使用uni.request({})，不用借助axios发请求，具体方式请看我的博客CSDN：DantinZhang  
（3）组件使用原生事件需要@click.native = "fun"（这其实是vue的特性）  
（4）路由跳转传参使用uni.navigateTo({url:''})，传的参数在另一个页面使用onLoad(参数)接收  
（4-1）路由跳转接收的参数是一个对象，里面包含了传过来的玩意儿们  

## 二、关于git的使用
### git的基本使用：
（1）项目目录下:git init初始化  
（2）git remote add 别名 http://...（项目链接）  
（3）git add .    
（4）git commit -m '提交备注'  
（5）git push 别名 分支名 
### git的一些常用命令
使用这个命令可以让某个文件脱离版本控制（修改时不跟随提交）。
```javascript
git rm -r --cached unpackage
```
使用`git status`可以查看当前git追踪状态   
使用`git log`可以查看提交日志
