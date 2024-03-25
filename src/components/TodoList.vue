<template>
  <TodoItem
    v-for="todo in todos"
    :key="todo.id"
    :todoProps="todo"
    @item-completed="markComplete"
    @delete-item="deleteTodo"
  />
  <!-- 
    v-bind có thể được viết tắt lại thành dấu 2 chấm (:)
      v-bind:key="todo.id" ==> :key="todo.id"
    v-on có thể được viết tắt lại thành dấu @
      v-on:item-completed="markComplete" ==> @item-completed="markComplete"
   -->
</template>

<script>
import { ref } from 'vue'
import TodoItem from './TodoItem'
export default {
  name: 'TodoList',
  components: { TodoItem },
  setup() {
    const todos = ref([
      {
        id: 1,
        title: 'Viec 1',
        completed: false
      },
      {
        id: 2,
        title: 'Viec 2',
        completed: false
      },
      {
        id: 3,
        title: 'Viec 3',
        completed: false
      }
    ])

    const markComplete = id => {
      todos.value = todos.value.map(todo => {
        if (todo.id === id) todo.completed = !todo.completed
        return todo
      })
    }

    // muốn chọc vào object todo để thay đổi các thuộc tính trong các thành phần có trong todos, ta sẽ phải truy cập vào ref với cú pháp
    // todos.value

    const deleteTodo = id => {
      todos.value = todos.value.filter(todo => todo.id !== id)
    }
    return {
      todos,
      markComplete,
      deleteTodo
    }
    /**
     * Ở đây, setup đóng vai trò khởi tạo trạng thái ban đầu của component TodoItem
     * Khi component này được khởi tạo, ref sẽ tạo ra một biến todos với giá trị ban đầu là một mảng chứa các việc cần làm
     * sau đó đưa todos vào object return để trả về
     * return ở đây đóng vai trò xuất khẩu (export) để sử dụng được trong template
     */
  }
}
</script>

<style></style>
