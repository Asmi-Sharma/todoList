<template>
  <div class="todo">
    <input
      type="text"
      class="todo-input"
      placeholder="Task to add"
      v-model="newTodo"
      @keyup.enter="addTodo"
    />
    <div
      v-for="(todo, index) in todosFiltered"
      :key="todo.id"
      class="todo-item"
    >
      <div class="todo-item-left">
        <input type="checkbox" v-model="todo.completed" />
        <div
          v-if="!todo.editing"
          @dblclick="editTodo(todo)"
          class="todo-item-label"
          :class="{ completed: todo.completed }"
        >
          {{ todo.title }}
        </div>
        <input
          v-else
          class="todo-item-edit"
          type="text"
          v-model="todo.title"
          @blur="doneEdit(todo)"
          @keyup.enter="doneEdit(todo)"
          @keyup.esc="cancelEdit(todo)"
          v-focus
        />
      </div>
      <div class="remove-item" @click="removeTodo(index)">
        &times;
      </div>
    </div>

    <div class="extra-container">
      <div>
        <label
          ><input
            type="checkbox"
            :checked="!anyRemaining"
            @change="checkAllTodos"
          />Check All</label
        >
      </div>
      <div>{{ remaining }}items left</div>
    </div>

    <div class="extra-container">
      <div class="button-display">
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">
          All
        </button>
        <button
          :class="{ active: filter == 'active' }"
          @click="filter = 'active'"
        >
          Active
        </button>
        <button
          :class="{ active: filter == 'completed' }"
          @click="filter = 'completed'"
        >
          Completed
        </button>
      </div>

      <div>
        <button v-if="showClearCompletedButton" @click="clearCompleted">
          Clear Completed
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: "",
      idForTodo: 3,
      beforeEditCache: "",
      filter: "all",
      todos: [
        {
          id: 1,
          title: "Learn git",
          completed: false,
          editing: false
        },
        {
          id: 2,
          title: "submit report",
          completed: false,
          editing: false
        }
      ]
    };
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length;
    },
    anyRemaining() {
      return this.remaining != 0;
    },
    todosFiltered() {
      if (this.filter == "all") {
        return this.todos;
      } else if (this.filter == "active") {
        return this.todos.filter(todo => !todo.completed);
      } else if (this.filter == "completed") {
        return this.todos.filter(todo => todo.completed);
      }
      return this.todos;
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0;
    }
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        return;
      }
      let edit = {
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
        editing: false
      };
      this.todos.push(edit);

      this.newTodo = "";
      this.idForTodo++;
    },
    editTodo(todo) {
      this.beforeEditCache = todo.title;
      todo.editing = true;
    },
    doneEdit(todo) {
      if (todo.title.trim() == "") {
        todo.title = this.beforeEditCache;
      }
      todo.editing = false;
    },
    cancelEdit(todo) {
      todo.title = this.beforeEditCache;
      todo.editing = false;
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    checkAllTodos() {
      this.todos.forEach(todo => (todo.completed = event.target.checked));
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);
    }
  }
};
</script>

<style scoped>
.todo {
  box-sizing: border-box;
  width: 30%;
  margin-left: 73vh;
  margin-right: 85vh;
}
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
}

.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.remove-item {
  cursor: pointer;
  margin-left: 14px;
}

.todo-item-left {
  display: flex;
  align-items: center;
}

.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}

  .todo-item-edit{
    font-size: 18px;
    color: #2c3e50;
    margin-left: 10px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    font-family: 'Times New Roman';
  }

  .todo-item-edit:focus{
    outline: none;
  }

.completed {
  text-decoration: line-through;
  color: gray;
}

.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 15px;
  border-top: 1px solid silver;
  padding-top: 12px;
  margin-bottom: 12px;
}

button {
  margin-left: 10px;
  border-radius: 15px;
  border: 1px solid black;
  font-size: 12px;
  padding: 10px;
  appearance: none;
}

:focus {
  outline: none;
}

.active {
  background: green;
}
</style>
