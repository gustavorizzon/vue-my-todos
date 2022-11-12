<script setup lang="ts">
import { computed, ref } from 'vue';

interface Todo {
  id: string;
  title: string;
  done: boolean;
}

const hideCompleted = ref<boolean>(true)
const newTodo = ref<string>("")
const todos = ref<Todo[]>([])

const filteredTodos = computed(() => {
  return hideCompleted.value 
    ? todos.value.filter((todo) => !todo.done)
    : todos.value
})

function createTodo(e: Event) {
  e.preventDefault();

  const title = newTodo.value.trim()
  
  if (title === '') return

  const todo: Todo = {
    id: Date.now().toString(16),
    title,
    done: false
  }

  todos.value.push(todo)
  newTodo.value = ''
}

function removeTodo(todoId: string): void {
  todos.value = todos.value.filter(todo => todo.id != todoId)
}

</script>

<template>
  <div class="todos-container">
    <h1>My To-Do's</h1>
  
    <hr>
  
    <form class="input-form" @submit="createTodo">
      <input type="text" v-model="newTodo">
      <button type="submit">Add To-Do</button>
    </form>

    <label for="hide-completed" class="hide-completed">
      <input type="checkbox" id="hide-completed" v-model="hideCompleted">
      <span>Hide Completed</span>
    </label>

    <ul v-if="filteredTodos.length" class="todo-list">
      <li v-for="todo in filteredTodos" :key="todo.id">
        <label :for="todo.id">
          <input type="checkbox" :id="todo.id" v-model="todo.done"/>
          <span>{{ todo.title }}</span>
        </label>
        <button type="button" @click="removeTodo(todo.id)">X</button>
      </li>
    </ul>
    <div v-else class="empty-todo-list">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75L11.25 15 15 9.75M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
      </svg>
      <span>
        All Done!
      </span>
    </div>
  </div>
</template>

<style scoped>
.todos-container {
  display: flex;
  flex-direction: column;
}

hr {
  max-width: 75%;
  margin: 1rem 0;
}

.input-form {
  display: flex;
  align-items: center;
  justify-content: center;
}

.input-form input {
  flex: 1;
  margin-right: 1rem;
  border-radius: 8px;
  padding: 0.6em 1.2em;
}

.hide-completed {
  display: flex;
  align-items: center;
  margin-top: .5rem;
  padding: .10rem;
}

.hide-completed input {
  margin-right: .6rem;
}

.todo-list {
  margin-top: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: start;
}

.todo-list li {
  display: flex;
  align-items: center;
}
.todo-list li button {
  background-color: rgb(168, 40, 40);
}

.todo-list li label {
  display: flex;
  align-items: center;
  flex: 1;
  margin-right: 1rem;
  background: rgba(255, 255, 255, 0.05);
  padding: 0.6em 1.2em;
  border-radius: 8px;
  transition: background-color .2s ease;
}

.todo-list li label:hover {
  background: rgba(255, 255, 255, 0.1);
}

.todo-list li + li {
  margin-top: .5rem;
}

.todo-list li label input {
  margin-right: 1rem;
  height: 20px;
  width: 20px;
}

.todo-list li label span {
  flex: 1;
  line-height: 20px;
}

.todo-list li label input[type=checkbox]:checked ~ span {
  text-decoration: line-through;
}

.empty-todo-list {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  padding: 1rem;
  color: #2ecc71;
}

.empty-todo-list svg {
  max-width: 200px;
}

.empty-todo-list span {
  margin-top: 1rem;
  font-size: 2rem;
}

@media (prefers-color-scheme: light) {
  .todo-list li label {
    background: rgba(0, 0, 0, 0.04);
  }

  .todo-list li label:hover {
    background: rgba(0, 0, 0, 0.08);
  }

  .todo-list li button {
    background-color: rgb(241, 87, 87);
    color: white;
  }
}
</style>
