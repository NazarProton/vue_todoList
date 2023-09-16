<script setup lang="ts">
import { PropType, defineProps, defineEmits } from 'vue';

const props = defineProps({
  todo: {
    type: Object as PropType<{
      id: string;
      text: string;
      completed: boolean;
    }>,
    required: true,
  },
});
const toggleToDo = () => {
  emit('toggleToDo', props.todo.id);
};

const removeToDo = () => {
  emit('removeToDo', props.todo.id);
};

const emit = defineEmits(['toggleToDo', 'removeToDo']);
</script>

<template>
  <li
    class="todo-item"
    :class="{ 'todo-item--done': todo.completed }"
    @click="toggleToDo"
  >
    <div class="todo-item__status">
      <i class="bi bi-check2"></i>
    </div>
    <span class="todo-item__text">{{ todo.text }}</span>
    <button class="todo-item__remove-button" @click.stop="removeToDo">
      <i class="bi bi-trash3"></i>
    </button>
  </li>
</template>
