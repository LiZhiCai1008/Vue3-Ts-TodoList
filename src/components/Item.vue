<template>
  <!-- 任务 -->
  <li class="todo-item" :class="[hover?'todo-active': '']" @mouseenter="mouseHandler(true)" @mouseleave="mouseHandler(false)">
    <label><input name="Fruit" type="checkbox" v-model="checked" /> {{ todo.title }}</label> 
    <button v-show="hover" class="todo-delete-btn" @click="deleteItem(index)">删除</button>
  </li>
</template>

<script lang="ts">
import { computed, defineComponent, ref } from 'vue';
import { Todo } from '@/types/Todo'

export default defineComponent({
  name: 'Item',
  props: {
    todo: {
      type: Object as () => Todo,
      require: true,
      default: () => {
        return {
          id: 1,
          title: '',
          checked: false
        }
      }
    },
    index: {
      type: Number,
      require: true
    },
    deleteTodo: {
      type: Function,
      require: true
    },
    updateTodo: {
      type: Function,
      require: true
    }
  },
  setup(props) {
    const hover= ref(false)
    // 鼠标划入划出
    const mouseHandler = (flag: boolean) => {
      hover.value = flag
    }
    // 删除Task
    const deleteItem = (idx: number): void => {
      if (!window.confirm('确认删除吗？')) {
        return 
      }
      props.deleteTodo && props.deleteTodo(idx)
    }
    // 计算属性
    const checked = computed({
      set(val) {
        props.updateTodo && props.updateTodo(props.todo, val)
      },
      get() {
        return props.todo.checked
      }
    })
    return {
      checked,
      mouseHandler,
      hover,
      deleteItem
    }
  }
});
</script>

<style scoped>
.todo-item {
  width: 100%;
  height: 32px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 12px;
  box-sizing: border-box;
  border: 1px solid #999;
  border-radius: 4px;
  margin-bottom: 10px;
}
.todo-active {
  background-color: pink;
  color: green;
}
.todo-delete-btn {
  width: 44px;
  height: 24px;
  font-size: 12px;
  color: #fff;
  background-color: lightcoral;
  border: none;
  outline: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>