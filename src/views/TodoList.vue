<template>
  <div>
    <h1>Todoリスト</h1>
    <form @submit.prevent="addTodo()">
      <input v-model="todo">
    </form>
    <div>
      <el-card :gutter="20" class="box-card" v-for="( todo, index ) in todos" :key="index">
        {{ todo }}
        <button @click="removeTodo(index)">完了</button>
      </el-card>
    </div>
  </div>
</template>
  
  <script>
  export default {
    name: 'TodoList',
    data(){
      return {
        todo: '',
        todos: [],
      }
    },
    methods: {
      addTodo(){
        this.todos.push(this.todo);
        this.todo = '';
      },
      removeTodo(index){
        this.todos.splice(index, 1);
      }
    }
  }
  </script>
  