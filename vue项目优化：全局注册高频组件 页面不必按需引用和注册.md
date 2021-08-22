

# vue项目优化：全局注册高频组件 页面不必按需引用和注册

高频组件可以定义在components文件夹下（一般是封装过的组件），单页面文件

#### 自动动态require组件：

借助webpack的require.context()创建模块上下文

#### 步骤：

在components文件夹添加一个叫global.js的文件，在这个文件里使用require.context 动态将需要的高频组件统统打包进来，然后在main.js文件中引入global.js的文件

#### 具体实现：

```
// global.js
import Vue from 'vue'
function changeStr (str) {
  return str.charAt(0).toUpperCase() + str.slice(1)
}
const requireComponent = require.context('./', false, /\.vue$/)
// 查找同级目录下以vue结尾的组件
const install = () => {
  requireComponent.keys().forEach(fileName => {
    let config = requireComponent(fileName)
    console.log(config) // ./child1.vue 然后用正则拿到child1
    let componentName = changeStr(
      fileName.replace(/^\.\//, '').replace(/\.\w+$/, '')
    )
    console.log(componentName);
    Vue.component(componentName, config.default || config)
  })
}
export default {
  install // 对外暴露install方法
}
```

```
// main.js
// 高频组件注册
import install from './components/global'
Vue.use(install)
```