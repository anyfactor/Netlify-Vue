<template>
  <div id="app">
    <h1>Todos</h1>
    <input type="text" v-model="todoName" @keyup.enter="addTodo">
    <ul>
      <li v-for="todo of todos" :key="todo.id">
        {{todo.title}}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

const baseURL = "https://jsonplaceholder.typicode.com/todos?_limit=5"

export default {
  name: 'app',
  data() {
    return {
      todos: [],
      todoName: ''
    }
  },
  async created() {
    try {
      const res = await axios.get(baseURL)

      this.todos = res.data;
    } catch(e) {
      console.error(e)
    }
  },
  methods: {
    addTodo(newTodo){
      const {title, completed} = newTodo;
      axios.post(baseURL, {
        title,
        completed
      })
        .then(res => this.todos = [...this.todos, newTodo])
        .catch(err => console.log(err))
    }
  }
}
</script>