### 1.初始化Vuetify项目后，页面如何去掉默认的滚动条？
```CSS
// 将html的overflow属性设置为auto
html, body, #app {
  height: 100%;
  width: 100%;
  overflow: auto;
}
```
