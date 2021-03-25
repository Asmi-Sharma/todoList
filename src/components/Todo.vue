

<template>
  <div class="list">
    <h1> {{ msg }}</h1>
    <input type="text" class="list" placeholder="Task to add" v-model="newTodo" @keyup.enter="addTodo">
    <div v-for="todo in todos" :key="todo.id" class="todo-item">
      <div class="todo-item-left">
        <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label">{{todo.title}}</div>
         <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
      </div>
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
      beforeEditCache: '',
      todos: [
        {
          'id': 1,
          'title' : 'Learn git',
          'completed': false,
          'editing' : false,
        },
        {
          'id': 2,
          'title' : 'submit report',
          'completed': false,
          'editing' : false,
        }
      ]
    }
  },
  directives: {
    focus: {
      inserted: function(el){
        el.focus()
      }
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
    },
    editTodo(todo){
        this.beforeEditCache = todo.title
        todo.editing = true
    },
    doneEdit(todo){
      if(todo.title.trim() == ''){
          todo.title = this.beforeEditCache
        }
      todo.editing = false
    },
    cancelEdit(todo){
      todo.title = this.beforeEditCache
      todo.editing = false
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


  .todo-item-label{
    padding: 20px;
    border: 1px solid white;
    margin-left: 100px;
  }

  .todo-item-edit{
    font-size: 18px;
    color: #2c3e50;
    margin-left: 10px;
    width: 30vw;
    padding: 10px;
    border: 1px solid #ccc;
    font-family: 'Times New Roman';
  }

  :focus{
    outline: none;
  }
</style>