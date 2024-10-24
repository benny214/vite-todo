<template>
  <h1>Tod App</h1>

  <form @submit.prevent="addTodo">
    <label>New Todo</label>
    <input 
      v-model="newTodo"
      type="text" />

      <button type="submit">Add new todo</button>
  </form>

  <h2>Todo list</h2>

  <ul>
    <li 
      v-for="todo in todos" 
      :key="todo.index">
      <span
        :class="{ done: todo.done }"
        @click="doneTodo(todo)">
        {{ todo.content }}
      </span>

      <button @click="removeTodo()">Remove </button>
    </li>
  </ul>

  <h4 v-if="todos.length === 0">No todos</h4>
</template>

<script>
import { ref } from 'vue'
export default {
  setup() {

    //data
    const newTodo = ref('');
    const todosData = JSON.parse(localStorage.getItem('todos')) || [];
    const todos = ref(todosData);

    //methods
    const addTodo = () => {
      if (newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value
        });

        newTodo.value = '';
        saveData();
      }
    };

    const doneTodo = (todo) => {
      todo.done = !todo.done;
      saveData();
    };

    const removeTodo = (idx) => {
      todos.value.splice(idx, 1);
      saveData();
    };

    const saveData = () => {
     const storageData = JSON.stringify(todos.value);
     localStorage.setItem('todos', storageData);
    };

    return {
      newTodo,
      todos,
      addTodo,
      doneTodo,
      removeTodo,
      saveData
    }
  },
}
</script>
