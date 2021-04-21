<template>
  <div class="todo-box">
    <Header :addTodo="addTodo" />
    <List :todos="todos" :deleteTodo="deleteTodo" :updateTodo="updateTodo"  />
    <Footer :todos="todos" :checkAllTodo="checkAllTodo" :deleteAll="deleteAll" />
  </div>
</template>

<script lang="ts">
import { defineComponent, toRefs, reactive } from 'vue';
import Header from '@/components/Header.vue';
import List from '@/components/List.vue';
import Footer from '@/components/Footer.vue';
import { Todo } from '@/types/Todo'

export default defineComponent({
  name: 'App',
  components: {
    Header,
    List,
    Footer
  },
  setup() {
    const state = reactive<{ todos: Todo[] }>({
      todos: []
    })
    // 添加任务
    const addTodo = (todo: Todo): void => {
      state.todos.unshift(todo)
    }
    // 删除任务
    const deleteTodo = (index: number): void => {
      state.todos.splice(index, 1)
    }
    // 设置选中状态
    const updateTodo = (todo: Todo, value: boolean): void => {
      todo.checked = value
    }
    // 全选
    const checkAllTodo = (value: boolean): void => {
      state.todos.forEach(todo => {
        todo.checked = value
      })
    }
    // 全部删除
    const deleteAll = (): void => {
      state.todos = []
    }
    return {
      ...toRefs(state),
      addTodo,
      deleteTodo,
      updateTodo,
      checkAllTodo,
      deleteAll
    }
  }
});
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
.todo-box {
  width: 40%;
  margin: 0 auto;
  border: 1px solid #999;
  border-radius: 4px;
  padding: 12px;
}
</style>
