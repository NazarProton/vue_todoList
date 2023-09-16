<template>
  <footer class="app-footer">
    {{ todos!.length - completedCount }} more to do, {{ completedCount }} done
  </footer>
</template>

<script setup lang="ts">
import { Todos } from '@/App.vue';
import { defineProps, computed, PropType } from 'vue';

const props = defineProps({
  todos: {
    type: Array as PropType<Todos>,
  },
});

const completedCount = computed(() => {
  return props.todos?.filter((todo) => todo.completed).length || 0;
});
</script>
