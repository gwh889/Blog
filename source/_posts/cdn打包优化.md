---
title: cdn打包优化
date: 2021-7-20
categories: 打包 
---

## cdn打包优化

首先需要在index.html文件中通过https://www.bootcdn.cn/网站查找项目所要引入的包文件，通过外部引入的方式引入，再在vue.config.js中写入以下代码，示例：

```js
module.exports = {
  publicPath: './',
  // outputDir: 'ribbon',
  // assetsDir: 'static',
  configureWebpack:{
    // 此处可以配置cdn的一些资源
    // 需要在index.html中引入cdn资源文件
    externals:{
      vue:'Vue',
      'vue-router':'VueRouter',
      axios:'axios',
      vuex:'Vuex',
      'element-ui':'ELEMENT',
      'iview':'iview',
      echarts:'echarts'
    }
  }
}
```

注意：通过外部引入cdn文件会遇到样式优先级的问题，如果有自己定义的外部样式文件，需要调高自己样式文件的优先级，否则自己写的样式可能会被覆盖掉。

再次打包发现自己的项目从23.3M变成了9.3M

