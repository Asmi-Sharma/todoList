

<template>
  <div class="list">
    <h1> {{ msg }}</h1>
    <input type="text" class="list" placeholder="Task to add" v-model="newTodo" @keyup.enter="addTodo">
    <div v-for="todo in todos" :key="todo.id" class="todo-item">
      {{todo.title}}
    </div>
  </div>
  
</template>

<script>

export default {
  name: 'Todo',
  props: {
    msg: String
  },
  data (){
    return {
      newTodo: '',
      ifForTodo: 3,
      todos: [
        {
          'id': 1,
          'title' : 'Learn git',
          'completed': false,
        },
        {
          'id': 2,
          'title' : 'submit report',
          'completed': false,
        }
      ]
    }
  },
  methods: {
    addTodo() {
        if(this.newTodo.trim().length == 0){
          return
        }
        this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
      })

      this.newTodo = ''
      this.idForTodo++
    }
  }
}

</script>
<style scoped>
  .list{
    align-content: center;
    padding: 10px 18px;
    font-size: 18px;
    margin-bottom: 16px;
  }
</style>