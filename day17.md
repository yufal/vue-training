## 你知道vue的双向数据绑定的原理吗？

Vue 实现 双向数据绑定 主要采用：数据劫持结合“发布-订阅”模式的方式，通过Object.defineProperty（）的 set 和 get，在数据变动时发布消息给订阅者触发监听。

```
Object.defineProperty(obj,prop,descriptor)
/*
　　参数解释：
　　　　obj: 要在其上定义属性的对象
　　　　prop：要定义或修改的属性的名称
　　　　descriptor：要定义或修改的属性描述符
*/
```

![](https://images2015.cnblogs.com/blog/938664/201705/938664-20170522225458132-1434604303.png)

在数据渲染时使用prop渲染数据，将prop绑定到子组件自身的数据上，修改数据时更新自身数据来替代prop，watch子组件自身数据的改变，触发事件通知父组件更改绑定到prop的数据。

Vue数据绑定这样做的好处是父组件数据改变时，不会修改存储prop的子组件数据，只是以子组件数据为媒介，完成对prop的双向修改。

Object.defineProperty 通过 getter 和 setter 劫持了对象赋值的过程，在这个过程中可以进行更新 dom 操作