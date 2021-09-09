<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.done"
        @change="handleCheck(todo.id)"
      />
      <!-- 如下代码也能实现功能，甚至不用添加函数事件，但是不太推荐，因为有点违反原则，因为修改了props -->
      <!-- <input type="checkbox" v-model="todo.done"/> -->
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <input
        type="text"
        :value="todo.title"
        v-show="todo.isEdit"
        @blur="handleBlur(todo, $event)"
        ref="inputTitle"
      />
    </label>
    <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
    <button
      class="btn btn-edit"
      @click="handleEdit(todo)"
      v-show="!todo.isEdit"
    >
      编辑
    </button>
  </li>
</template>

<script>
import pubsub from "pubsub-js";
export default {
  name: "MyItem",
  /*3.props：子组件接收父组件传递的函数
   props: ["todo", "checkTodo", "deleteTodo"],
   */
  props: ["todo"],

  methods: {
    //勾选or取消勾选
    handleCheck(id) {
      //  console.log(id);
      //数据在哪里，操作数据的方法就在哪里,所以去App.vue中操作
      //通知App组件将对应的todo对象的done值取反
      /*  4.props：调用这个函数勾选
        this.checkTodo(id);
      */

      // 3.全局事件总线传递数据
      this.$bus.$emit("checkTodo", id);
    },
    //删除
    handleDelete(id) {
      if (confirm("确定删除吗？")) {
        //通知App组件将对应的todo对象删除
        /*  4.props：调用这个函数删除
         this.deleteTodo(id);
         */
        /*3.全局事件总线传递数据
         this.$bus.$emit("deleteTodo", id);
          */
        //发布消息
        pubsub.publish("deleteTodo", id);
      }
    },
    // 编辑
    handleEdit(todo) {
      if (Object.prototype.hasOwnProperty.call(todo, "isEdit")) {
        //如果todo中有isEdit这个属性，则将其值改为true
        todo.isEdit = true;
      } else {
        //如果todo中没有isEdit这个属性，则设置改属性
        this.$set(todo, "isEdit", true);
      }
      // 在下一次 DOM 更新结束，更新模板后执行其指定的回调。
      // 因为要等handleEdit函数执行完毕后才会重新解析模板，而this.$refs.inputTitle.focus()要基于更新后的新DOM进行操作
      this.$nextTick(function () {
        // 自动获取焦点
        this.$refs.inputTitle.focus();
      });
      // 可以用定时器来达到目的
      // setTimeout(() => {
      //   this.$refs.inputTitle.focus();
      // });
    },
    //失去焦点回调（真正执行修改逻辑）
    handleBlur(todo, e) {
      todo.isEdit = false;
      if (!e.target.value.trim()) {
        return alert("输入不能为空");
      }
      this.$bus.$emit("updateTodo", todo.id, e.target.value);
    },
  },
};
</script>

<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
li:hover {
  background-color: #ddd;
}
li:hover button {
  display: block;
}
</style>