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
```vue
<template>
  <v-dialog
    v-model="show"
    max-width="320"
    persistent
  >
    <v-list
      class="py-2"
      color="primary"
      elevation="12"
      rounded="lg"
    >
      <v-list-item
        :prepend-icon="geticon"
        :title="gettitle"
      >
        <template v-slot:prepend>
          <div class="pe-4">
            <v-icon color="primary" size="x-large"></v-icon>
          </div>
        </template>

        <template v-slot:append>
          <v-progress-circular
            color="primary"
            indeterminate="disable-shrink"
            size="16"
            width="2"
          ></v-progress-circular>
        </template>
      </v-list-item>
    </v-list>
  </v-dialog>
</template>

<script setup>
import { ref } from 'vue'
const show = ref(false)
const gettitle = ref("")
const geticon = ref("")

function loading(title = "数据加载中...", icon = "mdi-table"){
  // console.log(title)
  // console.log(icon)
  gettitle.value = title;
  geticon.value = icon;
  show.value = true;
}

function close(){
  show.value = false;
}

defineExpose({
  loading,
  close
})
</script>

<style scoped>
</style>
```
> 3. 创建一个plugin，通过createApp的方式创建消息组件，再创建一个dom元素挂载消息组件实例。
>    `注意：如果消息组件中使用了第三方UI组件，记得在creatApp后，将UI库也挂载到实例上，不然会提示没有实例的错误。`
```javascript
// toast.js
import GlobalMessage from "@/components/GlobalMessage.vue";
import { createVuetify } from 'vuetify'
import { createApp } from 'vue'
// 插件安装方法
export default {
  install(app) {
    app.component('GlobalMessage', GlobalMessage);

    // 定义一个插件，用于全局调用消息提示
    app.config.globalProperties.$message = {
      show(options) {
        if(!options.timeout){
          options.timeout = 3000; // 3s 默认
        }
        const div = document.createElement('div');
        document.body.appendChild(div);
        const vuetify = createVuetify();
        const vm = createApp(GlobalMessage);
        vm.use(vuetify);
        const instance = vm.mount(div);
        instance.showMessage(options); // 调用组件方法显示消息
        setTimeout(() => {
          document.body.removeChild(div)
        },options.timeout)
      }
    };
  }
};
```
> 5. 在plugin中，将调用组件的展示方法挂载到全局使用，或者通过依赖注入的方式使用。
```javascript
export async function handleQueryData(handleApi, proxy){
  proxy.$loading();
  await handleApi(queryModel.value).then(res => {
    const data = res.data;
    queryModel.value.pageNum = data.pageIndex;
    queryModel.value.pageSize = data.pageSize;
    queryModel.value.totalNum = data.totalNum;
    queryModel.value.totalPage = data.totalPage;
    dataList.value = data.result;
    nextTick(() => {
      proxy.$hideloading();  //关闭loading
    })
  }).catch(err => {
      proxy.$hideloading();  //关闭loading
  })
}
```
