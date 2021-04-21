<template>
  <!-- 底部全选 -->
  <div class="todo-footer">
    <label>
      <input name="all" type="checkbox" v-model="isCheckAll" /> 
      <span>全选/全不选</span>
      &nbsp;
      <span>已完成<span style="color: green">{{ count }}</span>/全部{{ (todos || []).length }}</span>
    </label> 
    <button class="todo-delete-all-btn" @click="deleteAllTodos">清除所有任务</button>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue';
import { Todo } from '@/types/Todo'

export default defineComponent({
  name: 'Header',
  components: {},
  props: {
    todos: {
      type: Array as () => Todo[],
      require: true
    },
    checkAllTodo: {
      type: Function,
      require: true
    },
    deleteAll: {
      type: Function,
      require: true
    }
  },
  setup(props) {
    const count = computed((): number => {
      return (props.todos || []).reduce((pre, todo) => pre + (todo.checked ? 1 : 0), 0)
    })
    const isCheckAll = computed({
      get() {
        return Number(count.value) > 0 && count.value === (props.todos || []).length
      },
      set(val) {
        props.checkAllTodo && props.checkAllTodo(val)
      }
    })
    const deleteAllTodos = (): void => {
      props.deleteAll && props.deleteAll()
    }
    return {
      count,
      isCheckAll,
      deleteAllTodos
    }
  }
});
</script>

<style scoped>
.todo-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.todo-delete-all-btn {
  height: 24px;
  padding: 0 12px;
  font-size: 12px;
  color: #fff;
  background-color: lightcoral;
  border: none;
  outline: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>