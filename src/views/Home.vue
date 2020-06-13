<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <ToDos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import ToDos from '../components/ToDos';
import AddTodo from '../components/AddTodo';
// import axios from 'axios';
export default {
  name: 'Home',
  components: {
    ToDos,
    AddTodo
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      fetch(`https://jsonplaceholder.typicode.com/todos/${id}`, {
        method: 'DELETE'
      })
        .then(res => res.json())
        .then(json => this.todos = this.todos.filter(todo => todo.id !== json.id))
        .catch(err => console.log(err));
      
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;

      fetch('https://jsonplaceholder.typicode.com/todos', {
        method: 'POST',
        body: JSON.stringify({
          title: title,
          completed: completed
        }),
        headers: {
          "Content-type": "application/json; charset=UTF-8"
        }
      })
        .then(res => res.json())
        .then(json => this.todos = [...this.todos, json])
        .catch(err => console.log(err));
    }
  },
  created() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
      .then(res => res.json())
      .then(json => this.todos = json)
      .catch(err => console.log(err));
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
  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }
  .btn:hover {
    background: #666;
  }
</style>
