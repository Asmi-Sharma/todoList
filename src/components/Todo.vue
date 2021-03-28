<template>
  <div class="todo">
    <input type="text" class="todo-input" placeholder="Task to add" v-model="newTodo" @keyup.enter="addTodo">
    <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
      <div class="todo-item-left">
        <input type="checkbox" v-model=todo.completed>
        <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label" :class="{ completed: todo.completed}">{{todo.title}}</div>
        <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
      </div>
      <div class="remove-item" @click="removeTodo(index)">
        &times;
      </div>
    </div>

    <div class="extra-container">
      <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">Check All</label></div>
      <div>{{ remaining}}items left</div>
    </div>
  </div>
</template>

<script>
export default {
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
  computed: {
    remaining() {
      return this.todos.filter( todo=> !todo.completed).length
    },
    anyRemaining(){
      return this.remaining != 0
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
    },
     removeTodo(index){
      this.todos.splice(index, 1)
    }, 
    checkAllTodos(){
      this.todos.forEach((todo)=>todo.completed = event.target.checked)
    }
  }
}

</script>

<style scoped>


.todo{
  margin-left: 75vh;
  margin-right: 85vh;
}
  .todo-input{
    width: 100%;
    padding: 10px 18px;
    font-size: 18px;
    margin-bottom: 16px;
  }

.todo-item{
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.remove-item{
  cursor: pointer;
  margin-left: 14px ;
}

.todo-item-left{
  display: flex;
  align-items: center;
}

  .todo-item-label{
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
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

  .completed{
    text-decoration: line-through;
    color: gray;
  }

  .extra-container{
    display:flex;
    align-items: center;
    justify-content: space-between;
    font-size: 15px;
    border-top: 1px solid silver;
    padding-top: 12px;
    margin-bottom: 12px;  
  }

  button{
    font-size: 12px;
    background-color: white;
    appearance: none;
  }

  :hover{
    background-color: lightgreen;
  }

  :focus{
    outline: none;
  }

  .active{
    background: lightgreen;
  }
</style>