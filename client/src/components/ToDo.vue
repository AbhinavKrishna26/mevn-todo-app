<template lang="html">
  <div id='wrapper'>
    <h1>MEVN Stack Todo List</h1>
    <h4>Click on checkbox to delete Todo</h4>
    <!-- list todos and use checkbox inputs that call a delete method when clicked.  -->
    <ul>
      <li v-for='todo in todos' :key='todo._id'>
        <input
          id='checkbox'
          type='checkbox'
          @click='deleteTodo(todo._id)'>
        {{ todo.title }}
      </li>

      <form v-on:submit='addTodo($event)' class='add-todo'>
        <input type='text' placeholder='Enter Todo' v-model='newTodo'/>
        <input type='submit' value='Add To List'/>

      </form>
    </ul>

  </div>
</template>

<script>
import ToDoAPI from '@/services/ToDoAPI.js'

// data object returns existing Todo list then lifecycle hook 'mounted' used to show Todos
export default {
  data () {
    return {
      newTodo: '',
      todos: []
    }
  },
  mounted () {
    this.loadTodos()
  },
  methods: {
    async addTodo (evt) {
      evt.preventDefault() // prevents the form's default action from redirecting the page
      const response = await ToDoAPI.addTodo(this.newTodo)
      this.todos.push(response.data)
      this.newTodo = '' // clear the input field
    },
    deleteTodo (todoID) {
      ToDoAPI.deleteTodo(todoID)
      // remove the array element with the matching id
      this.todos = this.todos.filter(function (obj) {
        return obj._id !== todoID
      })
    },
    async loadTodos () {
      const response = await ToDoAPI.getToDos()
      this.todos = response.data
    }
  }
}
</script>

<style lang="css">
  ul, li {
    list-style: none;
    margin: 0;
    padding: 0;
    float: left;
  }
  li {
    clear: both;
  }
  #wrapper {
    width: 50%;
    margin: 0 auto;
    text-align: center;
  }

  h1, h4 {
    float: left;

  }
  .add-todo {
    clear: both;
    margin-top: 10px;
    float: left;
  }
  input[type='text'] {
    margin: 0 10px 0 0px;
    padding: 10px;
    border: none;
    outline: none;
    float: left;
    background-color: #8080c0;
    color: white;
    border-bottom: 1px solid white;
    font-size: 100%
  }
  ::placeholder {
    color: white;
  }
  input[type='submit'] {
    margin: 0;
    padding: 10px;
    border: none;
    outline: none;
    float: left;
    background-color: #487eb0;
    color: white;
    border-radius: 10px;
    font-size: 100%;
    cursor: pointer;
  }
</style>
