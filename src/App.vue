<template>
  <div id="app" class="container_customize">
    <div style="width: 90%">
      <Header /> 

      <AddTodo @add-todo="addTodo" />
      <p id="message"></p>

      <Todos v-bind:todos="todos" @del-todo="deleteTodo" /> 
    </div>
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
      if(confirm("Do you really want to delete?")){
        axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`);
        this.todos = this.todos.filter(todo => todo.id !== id);
        this.sessionMessage("Deleted Successfully");
      }
    },
    addTodo(newTodo) {
      const {title, completed} = newTodo;

      console.log(title);

      if(title != '') {
        document.getElementById('message').style.display = "none";
        document.getElementById('message').innerHTML = "";

        axios.post('https://jsonplaceholder.typicode.com/todos',{
          title,
          completed
        })
        .then(response=> this.todos = [...this.todos, response.data])
        .catch(error=> console.log(error))

      }else {
        this.sessionMessage("Please fill the text field");
      }
    },
    sessionMessage(text)
    {
        document.getElementById('message').style.display = "";
        document.getElementById('message').innerHTML = text;

        setTimeout(function(){
          document.getElementById('message').style.display = "none";
          document.getElementById('message').innerHTML = "";
        }, 2500)
    }
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

.container_customize {
  display: flex;
  justify-content: center;
  align-items: center;
}

 #message {
   background: tomato;
   color: #fff;
   text-align: center;
 }
</style>
