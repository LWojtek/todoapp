<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo" />
    <Todos :todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>

import Todos from './components/Todos.vue'
import Header from './components/layout/Header.vue'
import AddTodo from './components/AddTodo.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    Todos,
    Header,
    AddTodo
  },
  methods: {
    deleteTodo(id){
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then( this.todos = this.todos.filter(todo => todo.id !== id))
      .catch(err => console.log(err))     
    },
    addTodo(newTodo){
      const { title, completed } = newTodo;
      axios.post('https://jsonplaceholder.typicode.com/todos', {
       title,
       completed})
       .then(res =>  this.todos = [...this.todos, res.data])
       .catch(err => console.log(err))
     
    }
  },
  created(){
      axios.get('https://jsonplaceholder.typicode.com/todos?_limit=8')
      .then(res => this.todos = res.data)
      .catch(err => console.log(err));
  },
  data() {
    return {
      todos: [
      
      ]
    }
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
    font-family: Avenir, Helvetica, Arial, sans-serif;
  }

  .btn {
    display: inline-block;
    border: none;
    outline: none;
    background: #555;
    color: #fff;
    padding: 12px 20px;
    cursor: pointer;
    text-transform: uppercase;
    font-size: 18px;
  }

  .btn:hover {
    background: #666;
  }


</style>
