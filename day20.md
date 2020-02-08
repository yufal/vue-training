## 谈一谈你对vue响应式原理的理解？
![官网说明](https://vuejs.org/images/data.png)
```
Observer, 观察者，用来观察数据源变化.
Dep, 观察者和订阅者是典型的 一对多 的关系,所以这里设计了一个依赖中心,来管理某个观察者和所有这个观察者对应的订阅者的关系, 消息调度和依赖管理都靠它。
Watcher, 订阅者，当某个观察者观察到数据发生变化的时候，这个变化经过消息调度中心，最终会传递到所有该观察者对应的订阅者身上，然后这些订阅者分别执行自身的业务回调即可.
```
+ vue将data初始化为一个Observer并对对象中的每个值，重写了其中的get、set，data中的每个key，都有一个独立的依赖收集器。
+ 在get中，向依赖收集器添加了监听
+ 在mount时，实例了一个Watcher，将收集器的目标指向了当前Watcher
+ 在data值发生变更时，触发set，触发了依赖收集器中的所有监听的更新，来触发Watcher.update

![引用自CSDN](https://img-blog.csdn.net/2018070918431875?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2E0MTk0MTk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)