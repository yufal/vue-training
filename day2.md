## Vue组件data选项为什么必须是个函数而Vue的根实例则没有此限制？

1：因为组件的复用性，每用一次组件，就会有一个它的新实例被创建，每个实例可以维护一份被返回对象的独立的拷贝。如果data不是返回的函数则无法使复用的组件各自管理各自的data，因为它们引用了同一个对象。
2：在整个项目中vue的根实例只有1个，所以data既可以是返回对象，也可以是返回的函数，不会有其他影响。

>[资料源自vue官网](https://cn.vuejs.org/v2/guide/components.html#data-%E5%BF%85%E9%A1%BB%E6%98%AF%E4%B8%80%E4%B8%AA%E5%87%BD%E6%95%B0)


定义一个名为 button-counter 的新组件
```
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

组件是可复用的 Vue 实例，且带有一个名字：在这个例子中是 <button-counter>。我们可以在一个通过 new Vue 创建的 Vue 根实例中，把这个组件作为自定义元素来使用：
```
<div id="components-demo">
  <button-counter></button-counter>
</div>

new Vue({ el: '#components-demo' })
```

因为组件是可复用的 Vue 实例，所以它们与 new Vue 接收相同的选项，例如 data、computed、watch、methods 以及生命周期钩子等。仅有的例外是像 el 这样根实例特有的选项。你可以将组件进行任意次数的复用：
```
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```
注意当点击按钮时，每个组件都会各自独立维护它的 count。因为你每用一次组件，就会有一个它的新实例被创建。

# data 必须是一个函数
当我们定义这个 <button-counter> 组件时，你可能会发现它的 data 并不是像这样直接提供一个对象：
```
data: {
  count: 0
}
```
取而代之的是，一个组件的 data 选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝：
```
data: function () {
  return {
    count: 0
  }
}
```
*如果 Vue 没有这条规则，点击一个按钮就可能会像上述代码一样影响到其它所有实例！*