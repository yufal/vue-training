## 你知道nextTick的原理吗？

>在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

#### nextTick的由来：

由于VUE的数据驱动视图更新，是异步的，即修改数据的当下，视图不会立刻更新，而是等同一事件循环中的所有数据变化完成之后，再统一进行视图更新。

#### nextTick的触发时机：

在同一事件循环中的数据变化后，DOM完成更新，立即执行nextTick(callback)内的回调。

#### 应用场景：
需要在视图更新之后，基于新的视图进行操作。

以上出现了事件循环的概念，其涉及到JS的运行机制，包括主线程的执行栈、异步队列、异步API、事件循环的协作，此处不展开之后再总结。大致理解：主线程完成同步环境执行，查询任务队列，提取队首的任务，放入主线程中执行；执行完毕，再重复该操作，该过程称为事件循环。而主线程的每次读取任务队列操作，是一个事件循环的开始。异步callback不可能处在同一事件循环中。

#### 简单总结事件循环：

同步代码执行 -> 查找异步队列，推入执行栈，执行callback1[事件循环1] ->查找异步队列，推入执行栈，执行callback2[事件循环2]...
即每个异步callback，最终都会形成自己独立的一个事件循环。
结合nextTick的由来，可以推出每个事件循环中，nextTick触发的时机：
同一事件循环中的代码执行完毕 -> DOM 更新 -> nextTick callback触发
tips：本文的任务队列、消息队列、异步队列指同一个东西，均指macrotask queue。