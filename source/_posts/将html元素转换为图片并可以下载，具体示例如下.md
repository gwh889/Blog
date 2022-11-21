---
title: 将html元素转换为图片并可以下载
date: 2021-7-20
categories: demo 
---

将html元素转换为图片并可以下载，具体示例如下: 

```vue
<div class="previewPrint" id="previewPrint" ref="imageWrapper">
        <div v-for="item in barcodeList" :key="item.index" class="printBar">
          <div>料箱编码：AB0300001</div>
          <div class="printDescribe">
            <div>型号：AB-001</div>
            <div>重量(kg)：10.15</div>
          </div>
          <barcode :value="item.value" height="70">
            {{ item.value }}
          </barcode>
        </div>
</div>
//转换工具
import html2canvas from 'html2canvas'
```

```js
 //点击下载调用的函数
download(){
      html2canvas(this.$refs.imageWrapper, {
        backgroundColor: '#fff',
        scal: 2
      }).then((canvas) => {
        var context = canvas.getContext('2d');
        // 关闭抗锯齿
        context.mozImageSmoothingEnabled = false;
        context.webkitImageSmoothingEnabled = false;
        context.msImageSmoothingEnabled = false;
        context.imageSmoothingEnabled = false;

        let dataURL = canvas.toDataURL('image/png');
        this.createImgUrl = dataURL;
        this.fileDownload(dataURL);
      });
    },
     fileDownload (downloadUrl) {
      let aLink = document.createElement('a');
      aLink.style.display = 'none';
      aLink.href = downloadUrl;
      aLink.download = '料箱条形码.png';
      // 触发点击-然后移除
      document.body.appendChild(aLink);
      aLink.click();
      document.body.removeChild(aLink);
    },
```

