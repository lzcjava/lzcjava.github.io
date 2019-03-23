---
title: html2canvas div转为图片
date: 2019-03-23 14:46:26
tags:
---

安装

```
npm install html2canvas
```


```html

<div class="の" ref="の" id="の"></div>

```


```javascript

setImage () {
  let self = this
  let ref = this.$refs.の // 截图区域
  html2canvas(ref, {
    backgroundColor: '#DCDCDC'
  }).then((canvas) => {
    let dataURL = canvas.toDataURL("image/png")
    console.log("生成图片地址:" + dataURL)
  })
},


```