# 备忘
## 下载之后先运行npm install，然后npm run serve打开
## props：

​ (1).父组件 ==> 子组件 通信

​ (2).子组件 ==> 父组件 通信（要求父先给子一个函数，然后子调用这个函数去传递数据给父组件）
### 步骤:
   1. 在父组件定义函数
 methods: {
    addTodo(todoObj) {
      this.todos.unshift(todoObj);
    },
  },
   2.将父组件函数传递给子组件 
        <MyHeader :addTodo="addTodo"></MyHeader>
   3.子组件接收父组件传递的函数
  props: ["addTodo"],
   4.调用这个函数，通知App组件去添加一个todo对象
      this.addTodo(todoObj);      
  备注：已经全部被注释掉了
## webStorage：
存储内容大小一般支持5MB左右（不同浏览器可能还不一样）

浏览器端通过 Window.sessionStorage 和 Window.localStorage 属性来实现本地存储机制。

3. 相关API：

1. ```xxxxxStorage.setItem('key', 'value');```

    该方法接受一个键和值作为参数，会把键值对添加到存储中，如果键名存在，则更新其对应的值。
    
    值为字符串，如果存入的值为对象，则将其转换为字符串后存入

2. ```xxxxxStorage.getItem('person');```

   该方法接受一个键名作为参数，返回键名对应的值。对象转字符串后存入，取出后需将其重新转化为对象

3. ```xxxxxStorage.removeItem('key');```

   该方法接受一个键名作为参数，并把该键名从存储中删除。

4. ``` xxxxxStorage.clear()```

   该方法会清空存储中的所有数据。
4. 备注：

 SessionStorage存储的内容会随着浏览器窗口关闭而消失。
LocalStorage存储的内容，需要手动清除才会消失。
 ```xxxxxStorage.getItem(xxx)```如果xxx对应的value获取不到，那么getItem的返回值是null。
 ```JSON.parse(null)```的结果依然是null。
这两者的API用法一致
 ## 组件的自定义事件（MyHeader与MyFooter应用）

1. 一种组件间通信的方式，适用于：<strong style="color:red">子组件 ===> 父组件</strong>

2. 使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件（<span style="color:red">事件的回调在A中</span>）。

3. 绑定自定义事件：

    1. 第一种方式，在父组件中：```<Demo @atguigu="test"/>```  或 ```<Demo v-on:atguigu="test"/>```

    2. 第二种方式，在父组件中：

        ```js
        <Demo ref="demo"/>
        ......
        mounted(){
           this.$refs.xxx.$on('atguigu',this.test)
        }
        ```

    3. 若想让自定义事件只能触发一次，可以使用```once```修饰符，或```$once```方法。

4. 触发自定义事件：```this.$emit('atguigu',数据)```		

5. 解绑自定义事件```this.$off('atguigu')```

6. 组件上也可以绑定原生DOM事件，需要使用```native```修饰符。

7. 注意：通过```this.$refs.xxx.$on('atguigu',回调)```绑定自定义事件时，回调<span style="color:red">要么配置在methods中</span>，<span style="color:red">要么用箭头函数</span>，否则this指向会出问题！

## 全局事件总线（GlobalEventBus）（MyItem应用）

1. 一种组件间通信的方式，适用于<span style="color:red">任意组件间通信</span>。

2. 安装全局事件总线：

   ```js
   new Vue({
   	......
   	beforeCreate() {
   		Vue.prototype.$bus = this //安装全局事件总线，$bus就是当前应用的vm
   	},
       ......
   }) 
   ```

3. 使用事件总线：

   1. 接收数据：A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的<span style="color:red">回调留在A组件自身。</span>

      ```js
      methods(){
        demo(data){......}
      }
      ......
      mounted() {
        this.$bus.$on('xxxx',this.demo)
      }
      ```

   2. 提供数据：```this.$bus.$emit('xxxx',数据)```

4. 最好在beforeDestroy钩子中，用$off去解绑<span style="color:red">当前组件所用到的</span>事件。

## 消息订阅与发布（pubsub）

1.   一种组件间通信的方式，适用于<span style="color:red">任意组件间通信</span>。

2. 使用步骤：

   1. 安装pubsub：```npm i pubsub-js```

   2. 引入: ```import pubsub from 'pubsub-js'```

   3. 接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的<span style="color:red">回调留在A组件自身。</span>

      ```js
      methods(){
        demo(data){......}
      }
      ......
      mounted() {
        this.pid = pubsub.subscribe('xxx',this.demo) //订阅消息
      }
      ```

   4. 提供数据：```pubsub.publish('xxx',数据)```

   5. 最好在beforeDestroy钩子中，用```PubSub.unsubscribe(pid)```去<span style="color:red">取消订阅。</span>
## nextTick

1. 语法：```this.$nextTick(回调函数)```
2. 作用：在下一次 DOM 更新结束后执行其指定的回调。
3. 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行
## Vue封装的过度与动画
1. 作用：在插入、更新或移除 DOM元素时，在合适的时候给元素添加样式类名。

2. 写法：

  1. 准备好样式：

   1. 元素进入的样式：
```js
  v-enter：进入的起点
  v-enter-active：进入过程中
  v-enter-to：进入的终点
```
   2. 元素离开的样式：
```js
  v-leave：离开的起点
  v-leave-active：离开过程中
  v-leave-to：离开的终点
```
   3. 使用<transition>包裹要过度的元素，并配置name属性：
```
```
<transition name="hello">
	<h1 v-show="isShow">你好啊！</h1>
</transition>
```
备注：若有多个元素需要过度，则需要使用：<transition-group>，且每个元素都要指定key值。