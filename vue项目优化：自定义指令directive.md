# vue项目优化：自定义指令directive

1.自定义指令可以用来做权限管理

我们通常给一个元素添加 v-if / v-show 来做权限管理，但如果判断条件繁琐且多个地方需要判断，可以用全局自定义指令

具体实现：

```
// 新建个array.js文件，用于存放与权限相关的全局函数

// array.js
export function checkArray (key) {
  let arr = ['1', '2', '3', '4', 'demo']
  let index = arr.indexOf(key)
  if (index > -1) {
    return true // 有权限
  } else {
    return false // 无权限
  }
}
```

```
// 将array.js挂载到全局


// main.js
  import { checkArray } from "./common/array";
  Vue.directive("permission", {
    inserted (el, binding) {
      let permission = binding.value; // 获取到 v-permission的值
      if (permission) {
        let hasPermission = checkArray(permission);
        if (!hasPermission) { // 没有权限 移除Dom元素
          el.parentNode && el.parentNode.removeChild(el);
        }
      }
    }
  });

```

```
// 演示实例

<div class="btns">
    <button v-permission="'1'">权限按钮1</button>  // 会显示
    <button v-permission="'10'">权限按钮2</button>  // 无显示
    <button v-permission="'demo'">权限按钮3</button> // 会显示
  </div>
```

2.自由拖拽指令v-drag

```
directives: { //拖动函数
  drag: {
    // 指令的定义
    bind: function (el) {
    let odiv = el; //获取当前元素
    el.onmousedown = (e) => {
    //算出鼠标相对元素的位置
    let disX = e.clientX - odiv.offsetLeft;
    let disY = e.clientY - odiv.offsetTop;
    let left = '';
    let top = '';
    document.onmousemove = (e) => {
    //用鼠标的位置减去鼠标相对元素的位置，得到元素的位置
    left = e.clientX - disX;
    top = e.clientY - disY;
    //绑定元素位置到positionX和positionY上面
    //移动当前元素
    odiv.style.left = left + 'px';
    odiv.style.top = top + 'px';
                };
    document.onmouseup = (e) => {
    document.onmousemove = null;
    document.onmouseup = null;
                };
              };
            }
        }
    },
```