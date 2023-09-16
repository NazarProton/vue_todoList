<script setup lang="ts">
import AppHeader from './components/AppHeader.vue';
import AppFilters, { FilterOption } from './components/AppFilters.vue';
import AppToDoList from './components/AppToDoList.vue';
import AppAddToDo from './components/AppAddToDo.vue';
import AppFooter from './components/AppFooter.vue';
import { onMounted, ref } from 'vue';
import axios from 'axios';

export interface Todo {
  id: string;
  text: string;
  completed: boolean;
}
export type Todos = Todo[];

let todos = ref<Todos>([]);
let todosFiltered = ref<Todos>([]);
let activeFilterValue = ref<FilterOption>('All');

const handleAddTodo = (newToDo: Todo) => {
  todos.value.push(newToDo);
};

const toggleToDo = (id: string) => {
  const targetToDo = todosFiltered.value.find((todo) => todo.id === id);
  if (targetToDo) {
    targetToDo.completed = !targetToDo?.completed;
    changeFilter(activeFilterValue.value);
  }
};

const removeToDo = (id: string) => {
  if (todos.value) {
    todos.value = todos.value.filter((todo) => todo.id !== id);
  }
};

const fetchToDos = async () => {
  try {
    const response = await axios.get(
      'https://work-test-todo-default-rtdb.europe-west1.firebasedatabase.app/todos.json'
    );
    let allTodos: Todos = Object.values(response.data);
    todos.value = allTodos;
    todosFiltered.value = allTodos;
  } catch (error) {
    console.error('Помилка запиту:', error);
  }
};
const changeFilter = (value: FilterOption) => {
  activeFilterValue.value = value;
  if (activeFilterValue.value === 'Active') {
    todosFiltered.value = todos.value.filter((todo) => !todo.completed);
  } else if (activeFilterValue.value === 'Done') {
    todosFiltered.value = todos.value.filter((todo) => todo.completed);
  } else {
    todosFiltered.value = todos.value;
  }
};

onMounted(() => {
  fetchToDos();
});
</script>
<template>
  <div id="app">
    <AppHeader />
    <AppFilters
      @changeFilter="changeFilter"
      :activeFilter="activeFilterValue"
    />

    <main class="app-main">
      <AppToDoList
        :todos="todosFiltered"
        @removeToDo="removeToDo"
        :onToggleToDo="toggleToDo"
      />
      <AppAddToDo @addTodo="handleAddTodo" />
    </main>

    <AppFooter :todos="todosFiltered" />
  </div>
</template>

<style></style>
