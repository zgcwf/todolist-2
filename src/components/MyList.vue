<template>
  <ul class="todo-main">
    <!-- <MyItem
      v-for="todoObj of todos"
      :key="todoObj.id"
      :todo="todoObj"
      :checkTodo="checkTodo"
      :deleteTodo="deleteTodo"
    /> -->
    <!-- 将接收的父组件函数传递给子组件 -->
    <transition-group name="todo" appear="">
      <MyItem v-for="todoObj of todos" :key="todoObj.id" :todo="todoObj" />
    </transition-group>
  </ul>
</template>

<script>
import MyItem from "./MyItem.vue";
export default {
  name: "MyList",
  components: { MyItem },
  // props: ["todos", "checkTodo", "deleteTodo"],
  props: ["todos"],
};
</script>

<style scoped>
/*list*/
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}
.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
.todo-enter-active {
  animation: atguigu 0.5s linear;
}

.todo-leave-active {
  animation: atguigu 0.5s linear reverse;
}

@keyframes atguigu {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(0px);
  }
}
</style>