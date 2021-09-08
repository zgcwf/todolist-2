<template>
  <div class="todo-header">
    <input
      type="text"
      placeholder="请输入你的任务名称，按回车键确认"
      v-model.trim="title"
      @keyup.enter="add"
    />
  </div>
</template>

<script>
import { nanoid } from "nanoid";
// 先npm i nanoid下载，再引入nanoid，生成随机id，是个函数。
export default {
  name: "MyHeader",
  /*  
   3.props：子组件接收父组件传递的函数
   props: ["addTodo"],
  */
  data() {
    return {
      //收集用户输入的title
      title: "",
    };
  },
  methods: {
    add() {
      //校验数据
      if (!this.title.trim()) {
        // 如果this.title为空，取反，if判断语句中!this.title为非空，true。执行return语句，弹出alert（）函数
        // 如果this.title为非空，取反，if判断语句中!this.title为空，false。跳过return语句，执行下方语句
        return alert("输入不能为空");
      }
      // 不写else则执行完if语句之后依次执行下方语句
      //将用户的输入包装成一个todo对象
      const todoObj = { id: nanoid(), title: this.title, done: false };
      /*
      4.props：调用这个函数，子组件通知父组件App去添加一个todo对象
      this.addTodo(todoObj);
      */
      this.$emit("addTodo", todoObj);
      //清空输入
      this.title = "";
    },
  },
};
</script>



<style scoped>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
</style>

