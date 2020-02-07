## vue-router导航钩子有哪些？

一共有三种。
+ 第一种：是全局导航钩子：
```
router.beforeEach((to, from, next) => {
    // do someting
});
作用：跳转前进行判断拦截。
这三个参数 to 、from 、next 分别的作用：
to: Route，代表要进入的目标，它是一个路由对象
from: Route，代表当前正要离开的路由，同样也是一个路由对象
next: Function，这是一个必须需要调用的方法，而具体的执行效果则依赖 next 方法调用的参数
next()：进入管道中的下一个钩子，如果全部的钩子执行完了，则导航的状态就是 confirmed（确认的）
next(false)：这代表中断掉当前的导航，即 to 代表的路由对象不会进入，被中断，此时该表 URL 地址会被重置到 from 路由对应的地址
next(‘/’) 和 next({path: ‘/’})：在中断掉当前导航的同时，跳转到一个不同的地址
next(error)：如果传入参数是一个 Error 实例，那么导航被终止的同时会将错误传递给 router.onError() 注册过的回调
注意：next 方法必须要调用，否则钩子函数无法 resolved

router.afterEach((to, from) => {
    // do someting
});
```
+ 第二种：组件内的钩子,beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave
```
const File = {
    template: `<div>This is file</div>`,
    beforeRouteEnter(to, from, next) {
        // do someting
        // 在渲染该组件的对应路由被 confirm 前调用
    },
    beforeRouteUpdate(to, from, next) {
        // do someting
        // 在当前路由改变，但是依然渲染该组件是调用
    },
    beforeRouteLeave(to, from ,next) {
        // do someting
        // 导航离开该组件的对应路由时被调用
    }
}
```
+ 第三种：单独路由独享组件
```
cont router = new VueRouter({
    routes: [
        {
            path: '/file',
            component: File,
            beforeEnter: (to, from ,next) => {
                // do someting
            }
        }
    ]
});
至于他的参数的使用，和全局前置守卫是一样的
```


路由传递参数和传统传递参数是一样的，命名路由类似表单提交而查询就是url传递，在vue项目中基本上掌握了这两种传递参数就能应付大部分应用了。
1.命名路由搭配params，刷新页面参数会丢失
2.查询参数搭配query，刷新页面数据不会丢失
3.接受参数使用this.$router后面就是搭配路由的名称就能获取到参数的值