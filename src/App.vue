<template>
  <div class="app-container">
    <h1 class="title">
      Todo App
    </h1>

    <form @submit.prevent="addTodo" class="todo-form"> 
      <label class="todo-label">New todo</label>
      <input 
        v-model="newTodo"
        type="text"
        class="todo-input"
      >
      <button type="submit" class="add-btn">Add new todo</button>
    </form>

    <h2 class="subtitle">Todo List</h2>
   
    <ul class="todo-list">
      <li
        v-for="(todo, index) in todos"
        :key="index"
        class="todo-item"
      >
        <span 
          :class="['todo-text', {done: todo.done}]"
          @click="doneTodo(todo)">
          {{ todo.content }}
        </span>

        <button @click="removeTodo(index)" class="remove-btn">Remove</button>
      
      </li>
    </ul>

    <h4
      v-if="!todos.length"
      class="empty-list"
    >
      Empty list
    </h4>
  </div>
</template>

<script>
import { ref } from 'vue';
export default {
  setup() {
    const newTodo = ref('');
    const todosData = JSON.parse(localStorage.getItem('todos')) || [];
    const todos = ref(todosData);

    const addTodo = () => {
      if (newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value
        })
      }

      newTodo.value=''

      saveData()
    }

    const doneTodo = (todo) => {
      todo.done=!todo.done
    }

    const removeTodo = (idx) => {
      todos.value.splice(idx, 1)
    }

    const saveData = () => {
      const storageData = JSON.stringify(todos.value)
      localStorage.setItem('todos', storageData)
    }

    return {
      newTodo,
      todos,
      addTodo,
      doneTodo,
      removeTodo,
    }
  }
}
</script>
