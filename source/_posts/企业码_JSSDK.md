---
title: 企业码——JSSDK
date: 2021-7-20
categories: demo
---

JSSDK，是在企业码项目中使用，为了统计页面的一个访问量所进行的一个操作，不会跨域名去访问时，具体的实现方法是：

```vue
//vue在每次离开当前页面进行的一个方法调用
beforeRouteLeave(to, from, next) {
    if (to.name === "worktrace") {
      JSSDK_QYM.sendData({
        pageUrl: "http://qym.dtsum.com/worktrace",
      });
    } else if (to.name === "carddetail") {
      JSSDK_QYM.sendData({
        pageUrl: "http://qym.dtsum.com/carddetail",
      });
    }
    next();
  }
```

离开页面会发送https://qym.zj.gov.cn/servicePath/accessLog/qymLog这个请求代表设置成功。

总结5个入口文件的方法