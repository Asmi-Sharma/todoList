

<template>
  <div class="list">
    <h1> {{ msg }}</h1>
    <input type="text" class="list" placeholder="Task to add" v-model="newTodo">
  </div>
  
</template>

<script>

export default {
  name: 'Todo',
  props: {
    msg: String
  },
  
}

</script>