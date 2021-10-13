<template>
  <div id="app" class="flex ">
    <Header />

    <AddTodo @add-todo="addTodo" />

    <Todos v-bind:todos="todos" @del-todo="deleteTodo" /> 
  </div>
</template>

<script>
import Todos from "./components/Todos.vue";
import Header from "./components/layout/Header.vue";
import AddTodo from "./components/AddTodo.vue";
import axios from "axios";

export default {
  name: 'App',
  components: {
    Todos,
    Header,
    AddTodo
  },
  data(){
    return {
      todos : []
    }
  },
  methods: {
    deleteTodo(id) {
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      this.todos = this.todos.filter(todo => todo.id !== id)
    },
    addTodo(newTodo) {
      const {title, completed} = newTodo
      axios.post('https://jsonplaceholder.typicode.com/todos',{
        title,
        completed
      })
      .then(response=> this.todos = [...this.todos, response.data])
      .catch(error=> console.log(error))
      
    },
  },
  created() {
      axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
        .then(response=> this.todos = response.data)
        .catch(error=> console.log(error))
    }
}
</script>

<style>
 * {
   box-sizing: border-box;
   margin: 0;
   padding: 0;
 }

 body {
   font-family: Arial, Helvetica, sans-serif;
   line-height: 1.4;
 }
</style>
