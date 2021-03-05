<template>
  <section class="todoapp">

    <header class="header">
      <div class="top-header">
        <p>{{ date }}</p>
        <p class="title">Vue Js Tutorial To Do List</p>
        <span class="todo-count"><strong>{{ remaining }}</strong> tâches restantes</span>
      </div>

      <NewTodo v-on:addTodo="addTodo($event)"></NewTodo>


    </header>
    <input type="checkbox" class="toggle" v-model="allDone">

    <TodoList v-on:deleteTodo="deleteTodo($event)" :filteredTodo="filteredTodo"></TodoList>
    <button class="clear-completed" v-show="completed" @click.prevent="deleteCompleted">Supprimer les tâches finies</button>

    <footer class="footer" v-show="hasTodo">

      <ul class="filters">
        <li><a href="#" v-bind:class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
        <li><a href="#" v-bind:class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
        <li><a href="#" v-bind:class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>

      </ul>
    </footer>

  </section>
</template>

<script>

import NewTodo from "@/components/NewTodo";
import TodoList from "@/components/TodoList";
export default {
  name: "ToDoCard",
  components: {
    TodoList,
    NewTodo,

  },
  data () {
    return {
      day:["Dimanche","Lundi","Mardi","Mercredi","Jeudi","Vendredi","Samedi"],
      month:["Janvier","Février","Mars","Avril","Mai","Juin","Juillet","Août","Septembre","Octobre","Novembre","Décembre"],


      todo: [{
        name: 'Tâche de test',
        completed: false,
      }],
      filter: 'all',
    }
  },

  methods : {
    addTodo (value) {
      this.todo.push({
        completed: false,
        name: value,
      })
      this.newTodo = ''
    },

    deleteTodo (todos) {
      this.todo = this.todo.filter(i => i !== todos)
    },

    deleteCompleted () {
      this.todo = this.todo.filter(todos => !todos.completed)
    }
  },
  computed: {

    date: function (){
      const now = new Date()
      const day = now.toLocaleString("default", { weekday : "long"})
      const month = now.toLocaleString("default", {month: "long"})
      return `${day} ${now.getDay()} ${month}`

    },

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

    hasTodo () {
      return this.todo.length > 0
    },

    remaining () {
      return this.todo.filter(todos => !todos.completed).length
    },

    completed () {
      return this.todo.filter(todos => todos.completed).length
    },

    filteredTodo () {
      if(this.filter === 'todo'){
        return this.todo.filter(todos => !todos.completed)
      } else if (this.filter === 'done') {
        return this.todo.filter(todos => todos.completed)
      }
      return this.todo
    }
  }

}
</script>

<style src="./todo.css">

</style>