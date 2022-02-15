<template>
  <div>
    <h1>ToDo application</h1>
    <AddTodo 
      @add-todo="addTodo"
    />
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not completed</option>
    </select>
    <TodoList 
      v-if="filterTodo.length"
      v-bind:todos="filterTodo"
      @remove-todo="removeTodo"
    />
    <p v-else>No todos!</p>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList';
import AddTodo from '@/components/AddTodo';

export default {
  name: 'App',
  data() {
    return {
      todos: [
        // {id: 1, title: 'Buy bread', completed: false},
        // {id: 2, title: 'Buy oil', completed: false},
        // {id: 3, title: 'Buy eggs', completed: false},
      ],
      filter: 'all'
    }
  },
  components: {
    TodoList,
    AddTodo
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=10')
      .then(response => response.json())
      .then(json => this.todos = json)
  },
  // watch: {
  //   filter(value) {
  //     console.log(value);
  //   }
  // },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter(t => t.id !==id);
    },
    addTodo(todo) {
      this.todos.push(todo);
    }
  },
  computed: {
    filterTodo() {
      if (this.filter === 'all') {
        return this.todos;
      } else if (this.filter === 'completed') {
        return this.todos.filter(t => t.completed);
      } else {
        return this.todos.filter(t => !t.completed);
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

select {
    margin-bottom: 20px;
}
</style>