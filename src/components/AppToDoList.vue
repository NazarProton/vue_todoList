<script setup lang="ts">
import { defineProps, PropType, defineEmits } from 'vue';
import AppToDoItem from './AppToDoItem.vue';
import axios from 'axios';
import { Todo } from '@/App.vue';

const props = defineProps({
  todos: {
    type: Array as PropType<Todo[]>,
  },
});

const toggleToDo = (id: string) => {
  const targetToDo = props.todos?.find((todo) => todo.id === id);
  if (targetToDo) {
    axios
      .patch(
        `https://work-test-todo-default-rtdb.europe-west1.firebasedatabase.app/todos/${id}.json`,
        targetToDo
      )
      .then(() => {
        emit('toggleToDo', id);
      })
      .catch((error) => {
        if (error instanceof Error) {
          alert(error.message);
        }
      });
  }
};

const removeToDo = (id: string) => {
  axios
    .delete(
      `https://work-test-todo-default-rtdb.europe-west1.firebasedatabase.app/todos/${id}.json`
    )
    .then(() => {
      if (props.todos) {
        emit('removeToDo', id);
      }
    })
    .catch((error) => {
      if (error instanceof Error) {
        alert(error.message);
      }
    });
};

const emit = defineEmits(['removeToDo', 'toggleToDo']);
</script>

<template>
  <ul class="todo-list">
    <AppToDoItem
      v-for="todo in props.todos"
      :key="todo.id"
      :todo="todo"
      @toggleToDo="toggleToDo"
      @removeToDo="removeToDo"
    />
  </ul>
</template>
