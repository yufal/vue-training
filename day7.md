## vue为什么要求组件模板只能有一个根元素？

+ new vue({el:"#app"})
+ 单文件组件中，template下元素div，其实就是"树"状数据结构中的"根"
+ diff算法中要求，源码中patch.js里patchVnode()

tips:
template的三个特点：
隐藏性：该标签不会显示在页面的任何地方，无论里边有多少内容，它永远都是设置的隐藏状态(display=none)；
任意性：该标签可以写在任意地方，甚至是head,body,script标签内；
无效性：该标签里任何html内容都是无效的，不会起到任何作用，只能innerHTML来获取里边的内容。