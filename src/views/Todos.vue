<template>
  <div>
    <h2>Todos</h2>
    <router-link to="/">Home</router-link>
    <hr>
    <AddTodo
        @add-todo="addTodo"
    />
    <select v-model="filter">
      <option value="all">All</option>
      <option value="completed">Completed</option>
      <option value="not-completed">Not completed</option>
    </select>
    <hr/>
    <Loader v-if="loading"/>
    <TodoList
        v-else-if="filteredTodos.length"
        v-bind:todos="filteredTodos"
        @remove-todo="removeTodo"
    />
    <p v-else>No todos!</p>
  </div>
</template>

<script>
import AddTodo from "../components/AddTodo";
import TodoList from "../components/TodoList";
import Loader from "../components/Loader";

export default {
  name: 'Todos',
  data() {
    return {
      todos: [],
      loading: true,
      filter: 'all'
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
        .then(res => res.json())
        .then(json => {
          setTimeout(() => {
            this.todos = json;
            this.loading = false;
          }, 1000);
        })
  },
  // watch: {
  //   filter(value) {
  //
  //   }
  // },
  computed: {
    filteredTodos() {
      switch (this.filter) {
        case 'completed':
          return this.todos.filter(t => t.completed);
        case 'not-completed':
          return this.todos.filter(t => !t.completed);
        default:
          return this.todos;
      }
    }
  },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter(t => t.id !== id);
    },
    addTodo(newTodo) {
      this.todos.push(newTodo);
    }
  },
  components: {
    TodoList,
    AddTodo,
    Loader
  }
}
</script>

<style scoped>

</style>


