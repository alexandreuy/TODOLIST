<template>
  <div>
    <Todos v-model="todos"> </Todos>
  </div>
</template>

<script>
import Todos from './components/Todos'

export default {
  data () {
    return {
      todos: [
        {
          name: 'Demo',
          completed: true
        }
      ]
    }
  },
  methods: {
    addToDo () {
      this.todos.push({
        name: 'Jean',
        completed: false
      })
    }
  },
  components: {
    Todos
  }
}
</script>
