# vue-virtual-selcet

`一个vue2的虚拟列表选择器(virtual-select)`

![demo.gif](https://i.loli.net/2021/09/05/rUHQEFXlhe1598B.gif)

## 1.安装

`npm install vue-virtual-selcet`

## 2.注册

```js
import virtualSelcet from "vue-virtual-selcet";
Vue.use(virtualSelcet);
```

## 3.使用

```vue
<template>
  <virtual-selcet
    v-model="val"
    style="width: 300px"
    @change="change"
    :options="options"
    placeholder="请选择"
    :filterMethod="filterMethod"
  />
</template>

```

### Attributes

| 参数          | 说明                                                         | 类型               | 默认值                                                       |
| ------------- | ------------------------------------------------------------ | ------------------ | ------------------------------------------------------------ |
| options       | select的选项,例:[{value:"值",label:"标签"}]                  | Array              | []                                                           |
| placeholder   | 占位符                                                       | string             | 请选择                                                       |
| filterable    | 是否可搜索                                                   | boolean            | true                                                         |
| filter-method | 自定义搜索方法                                               | function           | function(val, optionList) { return optionList.filter((item) => item.label.toLowerCase().indexOf(val.toLowerCase()) == -1 ? false : true ); } |
| data-key      | 每个options数据对象中获取的唯一键,它的值**必须是唯一,且类型为数字或者字符串** | string \| function | function (option) { return option.value; }                   |

### Events

| 事件名称 | 说明                    | 回调参数       |
| -------- | ----------------------- | -------------- |
| change   | 选中值发生变化时触发    | 目前的选中值   |
| blur     | 当 input 失去焦点时触发 | (event: Event) |
| focus    | 当 input 获得焦点时触发 | (event: Event) |

