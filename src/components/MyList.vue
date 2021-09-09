<template>
  <ul class="todo-main">
    <!-- 将接收的父组件函数传递给子组件 -->
    <!-- <MyItem
      v-for="todoObj of todos"
      :key="todoObj.id"
      :todo="todoObj"
      :checkTodo="checkTodo"
      :deleteTodo="deleteTodo"
    /> -->

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
  animation: atguigu1 0.5s linear;
}

.todo-leave-active {
  animation: atguigu2 0.5s linear;
}

@keyframes atguigu1 {
  from {
    transform: translateY(-100%);
  }
  to {
    transform: translateY(0px);
  }
}
@keyframes atguigu2 {
  from {
    transform: translateY(0);
  }
  to {
    transform: translateY(100%);
  }
}
</style>