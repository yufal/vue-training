## 谈谈你对MVC、MVP和MVVM的理解？

随着web前端技术的发展，在借鉴成熟开发语言的前提下，mvc，mvp，mvvm三者是技术逐步发展下的产物，在不同时期下前端的一个最佳实践方案。

>MVC解释：
```
M：模型（Model）：数据保存
V：视图（View）：用户界面。
C：控制器（Controller）：业务逻辑

View 传送指令到 Controller
Controller 完成业务逻辑后，要求 Model 改变状态
Model 将新的数据发送到 View，用户得到反馈

接受用户指令时，MVC 可以分成两种方式。一种是通过 View 接受指令，传递给 Controller。
另一种是直接通过controller接受指令。
*tips:所有通信都是单向的。*
实例：Backbone.js
1. 用户可以向 View 发送指令（DOM 事件），再由 View 直接要求 Model 改变状态。
2. 用户也可以直接向 Controller 发送指令（改变 URL 触发 hashChange 事件），再由 Controller 发送给 View。
3. Controller 非常薄，只起到路由的作用，而 View 非常厚，业务逻辑都部署在 View。所以，Backbone 索性取消了 Controller，只保留一个 Router（路由器） 。
```

>MVP解释：
```
MVP 模式将 Controller 改名为 Presenter，同时改变了通信方向。

1. 各部分之间的通信，都是双向的。

2. View 与 Model 不发生联系，都通过 Presenter 传递。

3. View 非常薄，不部署任何业务逻辑，称为"被动视图"（Passive View），即没有任何主动性，而 Presenter非常厚，所有逻辑都部署在那里。
```

>MVVM解释：
```
MVVM 模式将 Presenter 改名为 ViewModel，基本上与 MVP 模式完全一致。
唯一的区别是，它采用双向绑定（data-binding）：View的变动，自动反映在 ViewModel，反之亦然。
```

总结：
+ 这三者都是框架模式，他们设计的目标都是为了解决Model和View的耦合问题；
+ MVC模式出现较早主要应用在后端，如Spring MVC、ASP.NET MVC等，在前端领域的早期也有应用，比如Backbone.js，它的优点是分层清晰，缺点是数据流混乱，灵活性带来的维护性问题。
+ MVP模式是在MVC之上进化而来，Presenter作为中间层负责MV通信，解决两者之间的耦合问题，但P层过于臃肿会导致后期维护成本的增加。
+ MVVM模式在前端领域有广泛的应用，它不仅解决了MV的耦合问题，还解决了维护两者之间映射关系的大量繁杂代码和DOM操作代码，在提高开发效率，代码可读性，出色的性能上都有不错的表现。