## 你知道vue中key的作用和工作原理吗？说说你对它的理解。

> 官方API知识点:

+ 1：在Vue.js中，key是6个特殊属性key, ref, is, slot, slot-scope, scope其中之一。
+ 2：key的值可以是number，也可以是string。
+ 3：key主要作用于Vue的virtual DOM算法，在diff new nodes list和old nodes list时，作为识别VNode的一个线索。
+ 4：如果不用key，Vue会用一种算法：最小化element的移动，并且会尝试尽最大程度在同适当的地方对相同类型的element，做patch或者reuse。
+ 5：如果使用了key，Vue会根据keys的顺序记录element，曾经拥有了key的element如果不再出现的话，会被直接remove或者destoryed。
+ 6：拥有同一个parent的children必须有unique keys。重复的key的导致render error。

[参考vue源码](https://github.com/vuejs/vue/blob/dev/src/core/vdom/patch.js) line-501-patchVnode