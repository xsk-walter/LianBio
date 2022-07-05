Myself Demo H5 - 脚手架源码待分析

###### 一、构建cli项目

```javascript
// 注L: 全局安装cnpm、nvm时，需要使用管理员权限打开终端下进行，否则关闭弹窗后会失效，也不知道为什么
// 脚手架 @vue/cli
// 利用脚手架创建uniapp项目，要求cli版本在3以上，node版本需要在12以上
一、cnpm install -g @vue/cli // npm安装版本2.9版本 需要使用cnpm 安装
二、vue create -p dcloudio/uni-preset-vue 项目名称 // 创建uniapp项目
三、cd 项目名称、npm install、npm run serve
```

###### 二、配置路由uni-simple-router

官网地址：https://hhyang.cn/v2/start/quickstart.html

个人博客：https://www.cnblogs.com/xsk-walter/p/16304853.html

2.1.1 快速安装

```javascript
npm install uni-simple-router
npm install uni-read-pages
```

2.1.2 配置vue.config.js

```javascript
//vue.config.js
const TransformPages = require('uni-read-pages')
const {webpack} = new TransformPages()
module.exports = {
	configureWebpack: {
		plugins: [
			new webpack.DefinePlugin({
				ROUTES: webpack.DefinePlugin.runtimeValue(() => {
					const tfPages = new TransformPages({
						includes: ['path', 'name', 'aliasPath']
					});
					return JSON.stringify(tfPages.routes)
				}, true )
			})
		]
	}
}
```

2.1.3 新建写入router.js  (脚手架创建项目，src文件夹下)

```javascript
// router.js
import {RouterMount,createRouter} from 'uni-simple-router';

const router = createRouter({
	platform: process.env.VUE_APP_PLATFORM,
     // 注： ROUTES是vue.config.js通过webpack的DefinePlugin方法注册了一个全局变量并挂载
	routes: [...ROUTES]
});
//全局路由前置守卫
router.beforeEach((to, from, next) => {
	next();
});
// 全局路由后置守卫
router.afterEach((to, from) => {
    console.log('跳转结束')
})

export {
	router,
	RouterMount
}
```

2.1.4 main.js文件引入router.js

2.1.5 路由页面配置

###### 三、uview组件库引入 uView2.x 版本

3.1 选择使用npm安装方式

```javascript
// 项目是由脚手架cli安装的，需要通过npm安装依赖scss
npm i sass -D
npm i sass-loader@10 -D

npm install uview-ui@2.0.31

// main.js
import uView from 'uview-ui'
Vue.use(uView)

/* uni.css */ 全局主题文件
@import 'uview-ui/theme.scss'

// uView基础样式 - App.vue 首行样式 lang="scss"
<style lang="scss">
@import "uview-ui/index.scss"
</style>

// 配置easycom组件模式 - pages.json中进行
{
    “easycom" : {
		"^u-(.*)": "uview-ui/components/u-$1.vue"
	},
    "pages": []
}
// 对于cli创建的项目需要单独配置vue.config.js
transpileDependencies: ['uview-ui']
```

注：需要安装babel-loader、sass-loader（node-sass、webpack）

* bebel:   npm install --save-dev babel-loader babel-core babel-preset-env webpack
  * 一个javasript编译器，用于旧浏览器或环境中将ECMAScript 2015+代码转换为向后兼容的js代码。@babel/cli将ES6转换为ES5。
  * 作用相对于一个交通指挥，只是在webpack打包时遇到js文件，交个babel处理，至于怎么处理，跟webpack就没有关系了，跟babel的配置有关。
* npm install webpack@4.16.5 webpack-cli@3.3.11 -g  我的本地环境
* npm install webpack-dev-server@3.11.0 -D
* TypeError: compiler.getInfrastructureLogger is not a function
* uview 2.x版本 使用总是报错，也不知道为什么，最后还是降到了1.x版本，而且不能使用cnpm 安装，需要使用npm安装

百问百答

微信公众号

H5 - 移动端

PC - 架构

Home页

后端开发

