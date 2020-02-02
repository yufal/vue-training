## 谈谈你对vue生命周期的理解？

>beforeCreate、created、beforeMount、mounted、beforeUpdate、updated、beforeDestroy、destroyed 8个钩子函数，创建=>挂载=>更新=>销毁

![vue官方的周期说明](http://doc.vue-js.com/images/lifecycle.png)

+ 首先创建一个实例，在new Vue()的对象过程中，先执行了init函数，在init的过程中先调用了beforeCreate钩子，然后再injections(注入)和reactivity(反应性)的时候再调用created。所以在init的时候事件已经调用了，我们在beforceCreate的时候千万不要去修改data里面赋值的数据，最早也要放在created里去做一些操作。

+ 当created完成后，它会判断instance(实例)里边是否包含有"el"option选项，如果没有的话它会调用vm.$mount(el)这个方法，然后再执行下一步；如果有的话，直接执行下一步。

+ 紧接着判断是否含有"template"这个选项，如果有的话，它会把template解析成一个render function，这是一个template编译过程，结果是解析成render函数：
```
render (h){
    return h('div',{},this.text)
}

解释一下，render函数里面的传参h就是Vue里面的createElement方法，return返回一个createElement方法，其中要传3个参数，第一个参数就是创建的div标签；第二个参数传了一个对象，对象里面可以是我们组件上面的props，或者是事件之类的东西；第三个参数就是div标签里面的内容，这里我们指向了data里面的text。
```

+ 使用render函数的结果和我们之前使用template解析出来的结果是一样的。render函数是发生在beforeMount和mounted之间的，这也从侧面说明了，在beforeMount的时候，$el还只是我们在HTML里面写的节点，然后到mounted的时候，它就把渲染出来的内容挂载到了DOM节点上。这中间的过程其实是执行了render function的内容。

+ 在使用.vue文件开发的过程当中，我们在里面写了template模板，在经过了vue-loader的处理之后，就变成了render function，最终放到了vue-loader解析过的文件里面。这样做有什么好处呢？原因是由于在解析template变成render function的过程，是一个非常耗时的过程，vue-loader帮我们处理了这些内容之后，当我们在页面上执行vue代码的时候，效率会变得更高。

+ beforeMount在有了render function的时候才会执行，当执行完render function之后，就会调用mounted这个钩子，在mounted挂载完毕之后，这个实例就算是走完流程了。

+ 后续的钩子函数执行的过程都是需要外部的触发才会执行。比如说有数据的变化，会调用beforeUpdate，然后经过Virtual DOM，最后updated更新完毕。当组件被销毁的时候，它会调用beforeDestory，以及destoryed。

+ 钩子函数，其实和回调是一个概念，当系统执行到某处时，检查是否有hook，有则回调。说的更直白一点，每个组件都有属性，方法和事件。所有的生命周期都归于事件，在某个时刻自动执行。