
# 项目介绍
> 该项目模仿市面上的大部分外卖app之类的软件，并且实现了部分功能，过程中运用了
vuejs的大部分api和思想，例如组件化开发、模块化开发，并且完成了组件包括, 头部组件、星级评价组件、商品组件、购物车数量
控制组件、购物车组件、商品详情组件、评价组件、评价类别组件、商家组件，其中商家组件、商品组件和商品详情组件使用better-scroll插件实现了
垂直方向滚动和水平方向上的滚动，该项目使用webpack作为项目构建工具，且大量使用es6语法，布局部分使用了sass预编译器，后台数据通过nodejs的express框架mock数据
前端通过vue-resource接受收据。

# 项目演示
![all](https://github.com/ShangMXiao/vueJsSeller/blob/master/Screenshots/all.gif)

### 1.头部组件[Github地址](https://github.com/ShangMXiao/vueJsSeller/blob/master/src/components/header/header.vue)
![](https://github.com/ShangMXiao/vueJsSeller/blob/master/Screenshots/header.png)
### 2.星级评价组件[Github地址](https://github.com/ShangMXiao/vueJsSeller/blob/master/src/components/star/star.vue)
![](https://github.com/ShangMXiao/vueJsSeller/blob/master/Screenshots/star.png)
### 3.商品组件[Github地址](https://github.com/ShangMXiao/vueJsSeller/blob/master/src/components/food/food.vue)
![](https://github.com/ShangMXiao/vueJsSeller/blob/master/Screenshots/good.png)
### 4.购物车数量控制组件[Github地址](https://github.com/ShangMXiao/vueJsSeller/blob/master/src/components/cartControl/cartControl.vue)
![](https://github.com/ShangMXiao/vueJsSeller/blob/master/Screenshots/cartcontrol.png)
### 5.购物车组件[Github地址](https://github.com/ShangMXiao/vueJsSeller/blob/master/src/components/shopcart/shopcart.vue)
![](https://github.com/ShangMXiao/vueJsSeller/blob/master/Screenshots/shopcart.png)
### 6.商品详情组件[Github地址](https://github.com/ShangMXiao/vueJsSeller/blob/master/src/components/food/food.vue)
![](https://github.com/ShangMXiao/vueJsSeller/blob/master/Screenshots/fooddetail.png)
### 7.评价组件[Github地址](https://github.com/ShangMXiao/vueJsSeller/blob/master/src/components/ratings/ratings.vue)
![](https://github.com/ShangMXiao/vueJsSeller/blob/master/Screenshots/rating.png)
### 8.评价类别组件[Github地址](https://github.com/ShangMXiao/vueJsSeller/blob/master/src/components/ratingSelect/ratingSelect.vue)
![](https://github.com/ShangMXiao/vueJsSeller/blob/master/Screenshots/selecttype.png)
### 9.商家组件[Github地址](https://github.com/ShangMXiao/vueJsSeller/blob/master/src/components/seller/seller.vue)
![](https://github.com/ShangMXiao/vueJsSeller/blob/master/Screenshots/seller.png)

# 技术栈
#### 1.vuejs2.0
#### 2.vue-router
#### 3.vue-resource
#### 4.nodejs
#### 6.express
#### 7.es6
#### 8.webpack
#### 9.sass
# selldemo

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
