<template>
  <!-- 头部 -->
  <input type="text" class="todo-input" placeholder="请输入待完成事项" v-model="todoTask" @keyup.enter="add">
  <button class="todo-add-btn" @click="add">添加</button>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { Todo } from '@/types/Todo'

export default defineComponent({
  name: 'Header',
  props: {
    addTodo: {
      type: Function,
      require: true
    }
  },
  setup(prop) {
    const todoTask = ref('')
    
    const add = () => {
      if (!todoTask.value.trim()) return
      const tempTodo: Todo = {
        id: new Date().getTime(),
        title: todoTask.value,
        checked: false
      }
      prop.addTodo && prop.addTodo(tempTodo)
      // 清空文本框
      todoTask.value = ''
    }
    return {
      todoTask,
      add
    }
  }
});
</script>

<style scoped>
.todo-input {
  width: 50%;
  height: 32px;
  padding: 0 12px;
  color: #000cef;
  outline: none;
}
.todo-input:focus {
  outline: none;
}
.todo-add-btn {
  width: 44px;
  margin-left: 12px;
  height: 28px;
  outline: none;
}
</style>
