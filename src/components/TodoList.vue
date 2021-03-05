<template>
  <div class="main">

    <ul class="todo-list">

      <li class="todo" v-for="todos in filteredTodo" :key="todos" v-bind:class="{completed: todos.completed}">

        <div class="view">

          <input type="checkbox" v-model="todos.completed" class="toggle">

          <label>{{ todos.name }}</label>

          <button class="destroy" @click.prevent="deleteTodo(todos)"></button>
        </div>

      </li>

    </ul>

  </div>
</template>

<script>
export default {
name: "TodoList",

  props: ['filteredTodo'],

  methods: {
    deleteTodo (todos) {
      this.$emit('deleteTodo', todos)
    },

    deleteCompleted () {
      this.todo = this.todo.filter(todos => !todos.completed)
    }
  },

  computed: {

    allDone: {
      get () {
        return this.remaining === 0
      },
      set (value) {


        this.todo.forEach(todos => {
          todos.completed = value
        })

      }
    },

    completed () {
      return this.todo.filter(todos => todos.completed).length
    },


  }
}
</script>

<style scoped>

</style>