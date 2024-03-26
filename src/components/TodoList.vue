<template>
  <AddTodo @add-todo="addTodo" />
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
import AddTodo from './AddTodo'
import axios from 'axios'
export default {
  name: 'TodoList',
  components: { TodoItem, AddTodo },
  setup() {
    const todos = ref([])

    const getAllTodos = async () => {
      try {
        const res = await axios.get(
          'https://jsonplaceholder.typicode.com/todos?_limit=5/'
        )
        todos.value = res.data
      } catch (error) {
        console.log(error)
      }
    }

    getAllTodos()

    const markComplete = id => {
      todos.value = todos.value.map(todo => {
        if (todo.id === id) todo.completed = !todo.completed
        return todo
      })
    }

    // muốn chọc vào object todo để thay đổi các thuộc tính trong các thành phần có trong todos, ta sẽ phải truy cập vào ref với cú pháp
    // todos.value

    const deleteTodo = async id => {
      try {
        await axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        todos.value = todos.value.filter(todo => todo.id !== id)
      } catch (error) {
        console.log(error)
      }
    }

    const addTodo = async newTodo => {
      try {
        const res = await axios.post(
          `https://jsonplaceholder.typicode.com/todos`,
          newTodo
        )
        todos.value.push(res.data)
      } catch (error) {
        console.log(error)
      }
    }
    return {
      todos,
      markComplete,
      deleteTodo,
      addTodo
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
