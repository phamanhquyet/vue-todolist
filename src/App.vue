<template>
  <div id="app">
    <AppHeader />
    <TodoList />
  </div>
</template>
<script>
import AppHeader from './components/AppHeader'
import TodoList from './components/TodoList'
export default {
  name: 'App',
  components: {
    AppHeader,
    TodoList
  }
}
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>
