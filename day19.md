## 什么是一个递归组件？

+ vue官方解释
>组件在它的模板内可以递归地调用自己，不过，只有当它有 name 选项时才可以：
```
name: 'unique-name-of-my-component'
```
当你利用Vue.component全局注册了一个组件, 全局的ID作为组件的 name 选项，被自动设置.
```
Vue.component('unique-name-of-my-component', {
  // ...
})
```
如果你不谨慎, 递归组件可能导致死循环:
```
name: 'stack-overflow',
template: '<div><stack-overflow></stack-overflow></div>'
```
上面组件会导致一个错误 “max stack size exceeded” ，所以要确保递归调用有终止条件 (比如递归调用时使用 v-if 并让他最终返回 false )。

+ 通过props从父组件拿到数据，递归组件每次进行递归的时候都会tree组件传递下一级children数据，整个过程结束之后，递归就完成了。当然这段代码只是简单的做了下递归组件的使用，对于树形结构的需求来说，我们一般只会去渲染一级的数据，当点击一级菜单时，再去渲染一级菜单下的结构，如此往复。那么v-if就可以实现我们的这个需求，当v-if设置为false时，递归组件将不会再进行渲染，设置为true时，继续渲染。