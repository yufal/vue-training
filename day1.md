## v-if和v-for哪个优先级高？如果两个同时出现，应该怎么优化得到更好的性能？

+ 当他们处于同一节点上时 Vue 处理指令v-for 比 v-if 具有更高的优先级;
+ 同时出现意味着 v-if将分别重复运行于每个 v-for循环中；
+ 如果不可避免的同时出现应新增template来进行v-if判断, 在子元素里面使用v-for， 避免增加无用的渲染开销。

参考资料[vue源码](https://github.com/vuejs/vue/blob/dev/src/compiler/codegen/index.js) line-55-genElement

```
错误示例
<ul>
  <li
      v-for="user in Users"
      :key="user.id"
      v-if="user.isActive"
  >
      {{ user.name }}
  </li>
</ul>
```

```
正确示例
<ul v-if="shouldShowUsers">
    <li
        v-for="user in activeUsers"
        :key="user.id"
    >
        {{ user.name }}
    </li>
</ul>

computed: {
    activeUsers: function () {
        return this.users.filter(function (user) {
            return user.isActive
        })
    }
}
```

我们将会获得如下好处：

过滤后的列表只会在 users 数组发生相关变化时才被重新运算，过滤更高效。
使用 v-for="user in activeUsers" 之后，我们在渲染的时候只遍历活跃用户，渲染更高效。
解藕渲染层的逻辑，可维护性 (对逻辑的更改和扩展) 更强。