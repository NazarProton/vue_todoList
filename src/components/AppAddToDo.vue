<script setup lang="ts">
import axios from 'axios';
import { ref, defineEmits } from 'vue';
import { nanoid } from 'nanoid';

let isFormVisible = ref<boolean>(false);
let inputed = ref<string>('');

const emit = defineEmits(['addTodo']);

const AddToDo = () => {
  const id = nanoid();
  const newToDo = { id, text: inputed.value, completed: false };
  axios
    .put(
      `https://work-test-todo-default-rtdb.europe-west1.firebasedatabase.app/todos/${id}.json`,
      newToDo
    )
    .then(() => {
      emit('addTodo', newToDo);
      inputed.value = '';
    })
    .catch((error) => {
      if (error instanceof Error) {
        alert(error.message);
      }
    });
};
const toggleOpenForm = () => {
  isFormVisible.value = !isFormVisible.value;
};
</script>

<template>
  <section class="add-todo">
    <form v-if="isFormVisible" class="add-todo__form" @submit.prevent="AddToDo">
      <button class="close-button" @click="toggleOpenForm" type="button">
        <i class="bi bi-x"></i>
      </button>
      <div class="text-input text-input--focus">
        <input v-model="inputed" class="input" />
      </div>
      <button class="button button--filled">Add task</button>
    </form>
    <button v-else class="add-todo__show-form-button" @click="toggleOpenForm">
      <i class="bi bi-plus-lg"></i>
    </button>
  </section>
</template>
