<template>
  <!-- 列表 -->
  <ul class="todo-list">
    <Item
      v-for="(todo, index) in todos"
      :key="index"
      :todo="todo"
      :deleteTodo="deleteTodo"
      :updateTodo="updateTodo"
      :index="index"
    />
  </ul>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Item from '@/components/Item.vue'

export default defineComponent({
  name: 'List',
  components: {
    Item
  },
  props: ['todos', 'deleteTodo', 'updateTodo']
});
</script>

<style scoped>
ul, li {
  list-style: none;
}
.todo-list {
  margin-top: 16px;
}
</style>