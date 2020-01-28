## 谈谈你对vue组件之间通信的理解？

+ props和$emit
```
父组件向子组件传递数据是通过prop传递的，子组件传递数据给父组件是通过$emit触发事件来做到的。

1).父组件传递了message数据给子组件，并且通过v-on绑定了一个getChildData事件来监听子组件的触发事件；
2).子组件通过props得到相关的message数据,最后通过this.$emit触发了getChildData事件。
```
+ $attrs和$listeners
```
第一种方式处理父子组件之间的数据传输有一个问题：如果父组件A下面有子组件B，组件B下面有组件C,这时如果组件A想传递数据给组件C怎么办呢？
如果采用第一种方法，我们必须让组件A通过prop传递消息给组件B，组件B在通过prop传递消息给组件C；要是组件A和组件C之间有更多的组件，那采用这种方式就很复杂了。Vue 2.4开始提供了$attrs和$listeners来解决这个问题，能够让组件A之间传递消息给组件C。
```
+ 中央事件总线
```
上面两种方式处理的都是父子组件之间的数据传递，而如果两个组件不是父子关系呢？这种情况下可以使用中央事件总线的方式。新建一个Vue事件bus对象，然后通过bus.$emit触发事件，bus.$on监听触发的事件。
```

+ provide和inject
```
父组件中通过provider来提供变量，然后在子组件中通过inject来注入变量。不论子组件有多深，只要调用了inject那么就可以注入provider中的数据。而不是局限于只能从当前父组件的prop属性来获取数据，只要在父组件的生命周期内，子组件都可以调用。
```

+ v-model
```
父组件通过v-model传递值给子组件时，会自动传递一个value的prop属性，在子组件中通过this.$emit(‘input’,val)自动修改v-model绑定的值
```

+  $parent和$children
```
Vue.component('child',{
    props:{
        value:String, //v-model会自动传递一个字段为value的prop属性
    },
    data(){
        return {
            mymessage:this.value
        }
    },
    methods:{
        changeValue(){
            this.$parent.message = this.mymessage;//通过如此调用可以改变父组件的值
        }
    },
    template:`
        <div>
            <input type="text" v-model="mymessage" @change="changeValue"> 
        </div>
})
Vue.component('parent',{
    template:`
        <div>
            <p>this is parent compoent!</p>
            <button @click="changeChildValue">test</button >
            <child></child>
        </div>
    `,
    methods:{
        changeChildValue(){
            this.$children[0].mymessage = 'hello';
        }
    },
    data(){
        return {
            message:'hello'
        }
    }
})
var app=new Vue({
    el:'#app',
    template:`
        <div>
            <parent></parent>
        </div>
    `
})
```

+ boradcast和dispatch
```
vue1.0中提供了这种方式，但vue2.0中没有，但很多开源软件都自己封装了这种方式，比如min ui、element ui和iview等。
比如如下代码，一般都作为一个mixins去使用, broadcast是向特定的父组件，触发事件，dispatch是向特定的子组件触发事件，本质上这种方式还是on和on和emit的封装，但在一些基础组件中却很实用。

function broadcast(componentName, eventName, params) {
  this.$children.forEach(child => {
    var name = child.$options.componentName;

    if (name === componentName) {
      child.$emit.apply(child, [eventName].concat(params));
    } else {
      broadcast.apply(child, [componentName, eventName].concat(params));
    }
  });
}
export default {
  methods: {
    dispatch(componentName, eventName, params) {
      var parent = this.$parent;
      var name = parent.$options.componentName;
      while (parent && (!name || name !== componentName)) {
        parent = parent.$parent;

        if (parent) {
          name = parent.$options.componentName;
        }
      }
      if (parent) {
        parent.$emit.apply(parent, [eventName].concat(params));
      }
    },
    broadcast(componentName, eventName, params) {
      broadcast.call(this, componentName, eventName, params);
    }
  }
}
```

+ vuex处理组件之间的数据交互
```
如果业务逻辑复杂，很多组件之间需要同时处理一些公共的数据，这个时候才有上面这一些方法可能不利于项目的维护，vuex的做法就是将这一些公共的数据抽离出来，然后其他组件就可以对这个公共数据进行读写操作，这样达到了解耦的目的。
```