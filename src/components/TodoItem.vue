<template>
  <div class="todo-item" v-bind:class="{'is-complete':todo.completed}">
    <p>
      <input type="checkbox" @change="onChange" />
      {{ todo.title }}
      <button @click="$emit('del-todo', todo)" class="del">x</button>
    </p>
  </div>
</template>
 
<script>
export default {
  name: "TodoItem",
  props: ["todo"],
  methods: {
    onChange() {
      this.markComplete();
      this.emitChange();
    },
    emitChange() {
      this.$emit("edit-todo", this.todo);
    },
    markComplete() {
      this.todo.completed = !this.todo.completed;
    }
  }
};
</script>

<style scoped>
.todo-item {
  background: #f4f4f4;
  padding: 12px;
  border-bottom: 1px #ccc dotted;
  text-transform: capitalize;
  font-size: 1em;
}

.is-complete {
  text-decoration: line-through;
}

.del {
  background: #ff0000;
  color: #fff;
  border: none;
  padding: 5px 9px;
  border-radius: 50%;
  cursor: pointer;
  float: right;
}
</style>