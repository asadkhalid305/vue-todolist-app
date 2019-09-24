<template>
  <div>
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" v-on:edit-todo="editTodo" />
  </div>
</template>

<script>
import Axios from "axios";
import Todos from "../components/Todos";
import AddTodo from "../components/AddTodo";

const baseUrl = "https://jsonplaceholder.typicode.com/todos";
const todoLimit = `limit=10`;
let index = null;

export default {
  name: "home",
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    };
  },

  methods: {
    //create
    addTodo(todo) {
      Axios.post(`${baseUrl}`, todo)
        .then(res => (this.todos = [res.data, ...this.todos]))
        .catch(err => console.log(err));
    },

    //retrieve
    getTodos() {
      Axios.get(`${baseUrl}?_${todoLimit}`)
        .then(res => (this.todos = res.data))
        .catch(err => console.log(err));
    },

    //update
    editTodo(todo) {
      Axios.put(`${baseUrl}/${todo.id}`, todo)
        .then(res => {
          index = this.todos.findIndex(item => item.id === todo.id);
          if (index > -1) this.todos[index] = todo;
        })
        .catch(err => console.log(err));
    },

    //delete
    deleteTodo(todo) {
      Axios.delete(`${baseUrl}/${todo.id}`)
        .then(
          res => (this.todos = this.todos.filter(item => item.id !== todo.id))
        )
        .catch(err => console.log(err));
    }
  },
  created() {
    this.getTodos();
  }
};
</script>

<style scoped></style>