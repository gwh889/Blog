---
title: keep-alive
date: 2021-7-20
categories: VUE 
---

需求：系统列表页设置好的筛选条件，离开本页面，下次再点进来，不会被清空。

解决的方法：使用keep-alive

使用方法： 

1.需要保存数据的组件需要用keep-alive作为外层标签，可以放在app.vue中

```
<keep-alive>
    <!-- 需要缓存的视图组件 -->
    <router-view v-if="$route.meta.keepAlive"></router-view>
</keep-alive>
<!-- 不需要缓存的视图组件 -->
<router-view v-if="!$route.meta.keepAlive"></router-view>
```

2.在路由文件中添加要进行缓存的标识，keepAlive为true时进行缓存

```vue
{
    path: "/materialManage/materialOutput", // 原材料出仓
    name: "materialOutput",
    component: (resolve) => require(["../views/pages/materialOutput"], resolve),
    meta: {
      title: "原材料出仓",
      keepAlive: true,
    }
},
```

### 原理和理解

1.keep-alive不会生成真正的DOM节点。Vue在初始化生命周期的时候，为组件实例建立父子关系会根据abstract属性决定是否忽略某个组件。在keep-alive中，设置了abstract:true，那Vue就会跳过该组件实例

2.keep-alive的生命周期

- 初次进入时：created > mounted > activated；退出后触发 deactivated
- 再次进入：会触发 activated；事件挂载的方法等，只执行一次的放在 mounted 中；组件每次进去执行的方法放在 activated 中

注：activated只在被 keep-alive 缓存的组件激活时调；deactivated被 keep-alive 缓存的组件停用时调用。

3.属性：

- `include` - 字符串或正则表达式。只有名称匹配的组件会被缓存。

- `exclude` - 字符串或正则表达式。任何名称匹配的组件都不会被缓存。
- `max` - 数字。最多可以缓存多少组件实例。一旦这个数字达到了，在新实例被创建之前，已缓存组件中最久没有被访问的实例会被销毁掉。

实例：结合澳亚项目，顶部菜单存储在vuex，如果在顶部菜单栏被关掉，之前的信息不进行存储

```js
watch: {
    $route(to, from) {
      const routerName = this.$store.state.tagsView.visitedViews.find(item => {
        return item.name === from.name;
      });
	 if ("keepAlive" in to.meta) {
         to.meta.keepAlive=true;  //解决第一次进入缓存无效的问题
       }
      if ("keepAlive" in from.meta) {
        if (routerName === undefined) {
          from.meta.keepAlive = false;
        } else {
          from.meta.keepAlive = true;
        }
      }
    }
  }
```

总结： 一般情况下，我们都可以通过设置keepAlive为false清除缓存，但对于查看详情，类似于首检查看详情时，详情页会保存记录，下次再查看另一个的详情，不需要上一次的缓存，解决的办法如下： 

```js
//在首检详情页面添加如下代码
beforeRouteLeave(to, from, next) {
    Array.prototype.remove = function(val) {
      var index = this.indexOf(val);
      if (index > -1) {
        this.splice(index, 1);
      }
    }; //在数组原型上添加移除指定元素（非指定方法）
    //his.$vnode.parent.componentInstance.cache是keep-alive存放的缓存
    for (const key in this.$vnode.parent.componentInstance.cache) {
      //删除firstInspectionDetail页面中的缓存，在该页面被完全关闭时，也就是不存在顶部导航栏里
      if (
        this.$vnode.parent.componentInstance.cache[key].elm.className ==
          "firstInspectionDetail" &&
        this.$store.state.tagsView.visitedViews.find(item => {
          return item.name == "firstInspectionDetail";
        }) == undefined
      ) {
        delete this.$vnode.parent.componentInstance.cache[key];
        this.$vnode.parent.componentInstance.keys.remove(key);
      }
    }
    next();
  },
```

