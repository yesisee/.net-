### 1.初始化Vuetify项目后，页面如何去掉默认的滚动条？
```CSS
// 将html的overflow属性设置为auto
html, body, #app {
  height: 100%;
  width: 100%;
  overflow: auto;
}
```

### 2.封装全局消息提示
> 1. 先封装一个消息提示的组件。
> 2. 创建一个plugin，通过createApp的方式创建消息组件，再创建一个dom元素挂载消息组件实例。
>    `注意：如果消息组件中使用了第三方UI组件，记得在creatApp后，将UI库也挂载到实例上，不然会提示没有实例的错误。`
> 4. 在plugin中，将调用组件的展示方法挂载到全局使用，或者通过依赖注入的方式使用。
