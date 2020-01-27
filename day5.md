## 谈一谈对vue组件化的理解？

> 组件化定义、优点，使用场景和注意事项，同时强调vue中组件化的一些特点。
```
组件的定义：
    vue.component('componentName',{tamplate:'<div>this is a componnet</div>'})
src/core/global-api/asset.js

    <template>
        <div>this is a componnet</div>
    </template>
webpack会使用vue-loader编译template为render函数，最终导出的依然是组件配置对象。


组件优化点：
lifecycle.js - mountComponnet()
组件、watcher、渲染函数和更新函数之间的关系


组件化实现：
构造函数,src/core/global-api/extend.js
实例化及挂载,src/core/vdom/patch.js -createEle()
```

+ 组件是独立和可复用的代码组织单元，组件系统是vue的核心特性之一，它使开发者使用小型，独立和常用可复用组件构建大型应用；
+ 组件化开发能大幅提高应用开发效率、测试性、复用性等；
+ 组件使用按分类有页面组件、业务组件、通用组件；
+ vue的组件是基于配置的，我们通常编写的组件是组件的配置文件而非组件，框架后续会生成其构造函数，他们基于vuecommponent，扩展于vue；
+ vue中常见的组件化技术有：属性prop，自定义事件，插槽等，他们主要用于组件通信，扩展等；
+ 合理的规划使用组件，有助于提升应用的性能；
+ 组件是高内聚低耦合的；
+ 组件应该遵循单向数据流的原则；