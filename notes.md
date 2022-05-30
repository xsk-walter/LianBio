一、tabbar：uni-app 配置  --- 动态配置方法

* 动态tabbar：原生uniapp没有动态配置的方法； uview的tabbar组件可以实现

```vue
// 1⃣ 将list tabbar 存在vuex中
this.$store.state.userInfo.tabbarlist // 获取

// 2⃣ 模板部分格式要和 正文内容并列
<view>
	<view class="content">
  	...
  </view>
  <u-tabbar :list="$store.state.userInfo.tabbarlist" @change="changeTb"></u-tabbar>
</view>

// 3⃣ 动态添加
changeTb(index) {
	this.$Router.pushTab(this.$store.state.userInfo.tabbarlist[index].pagePath)
}
```

* 注：该方式是把uniapp的原生tabbar隐藏；用到状态管理中的tabbarlist数据是一个结构参数完整的tabbar配置，但是tabbar页面可能是不完整的，由用户权限决定