/** 11:42 https://www.youtube.com/watch?v=SgtS7jWF2O4&list=PLjwdMgw5TTLW-mAtlR46VajrKs4dep3y0&index=11 */
<template>
  <section class="todoapp">
    <header class="header">
      <h1>Todos</h1>
      <input type="text" placeholder="Ajouter une tâche" class="new-todo" v-model="newTodo" @keyup.enter="addsTodo"/>
    </header>
    <div class="main">
      <input id="toggle-all" type="checkbox" class="toggle-all" v-model="allDone">
      <label for="toggle-all"></label>
      <ul class="todo-list">
        <li class="todo" v-for="task in filteredTodos" :key="task.name" :class="{completed: task.completed, editing: task === editing}">
          <div class="view">
            <input type="checkbox" v-model="task.completed" class="toggle">
            <label @dblclick="editTodo(task)">{{ task.name }}</label>
            <button class="destroy" @click.prevent="deleteTodo(task)"></button>
          </div>
          <input type="text" class="edit" v-model="task.name" @keyup.enter="doneEdit" @keyup.esc="cancelEdit" v-focus="task === editing" @blur="doneEdit">
        </li>
      </ul>
    </div>
    <footer class="footer" v-show="hasTodos">
      <span class="todo-count" v-if="remaining > 1"> <strong> {{ remaining }}</strong> Tâches restantes</span>
      <span class="todo-count" v-else> <strong> {{ remaining }}</strong> Tâche restante</span>
      <ul class="filters">
        <li> <a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'"> Toutes</a></li>
        <li> <a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'"> A faire </a></li>
        <li> <a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'"> Faites</a></li>
      </ul>
      <button class="clear-completed" @click.prevent="deleteCompleted" v-show="doneToDo"> Supprimer tâches terminées </button>
    </footer>
  </section>
</template>

<script>
import Vue from 'vue'

export default {
  props: {
    value: {type: Array, default () { return [] }}
  },
  data () {
    return {
      todos: this.value,
      newTodo: '',
      filter: 'all',
      editing: null,
      oldTodo: ''
    }
  },
  watch: {
    value (value) {
      this.todos = value
    }
  },
  methods: {
    addsTodo () {
      if (this.newTodo !== '') {
        this.todos.push({
          completed: false,
          name: this.newTodo
        })
        this.newTodo = ''
      }
    },
    deleteTodo (task) {
      this.todos = this.todos.filter(i => i !== task)
      this.$emit('input', this.todos)
    },
    deleteCompleted () {
      this.todos = this.todos.filter(todo => !todo.completed)
      this.$emit('input', this.todos)
    },
    editTodo (todo) {
      this.editing = todo
      this.oldTodo = todo.name
    },
    doneEdit () {
      this.editing = null
    },
    cancelEdit () {
      this.editing.name = this.oldTodo
      this.doneEdit()
    }
  },
  computed: {
    allDone: {
      get () {
        return this.remaining === 0
      },
      set (value) {
        this.todos.forEach(todo => {
          todo.completed = value
        })
      }
    },
    remaining () {
      return this.todos.filter(todo => !todo.completed).length
    },
    doneToDo () {
      return this.todos.filter(todo => todo.completed).length
    },
    hasTodos () {
      return this.todos.length > 0
    },
    filteredTodos () {
      if (this.filter === 'todo') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed)
      } return this.todos
    }
  },
  directive: {
    focus (el, value) {
      if (value) {
        Vue.nextTick(_ => {
          el.focus()
        })
      }
    }
  }
}
</script>

<style src="./todos.css">
</style>

