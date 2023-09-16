<template>
  <aside class="app-filters">
    <section class="toggle-group">
      <button
        v-for="filter in filters"
        :key="filter"
        @click="changeFilter(filter)"
        class="button"
        :class="{ 'button--primary': props.activeFilter === filter }"
      >
        {{ filter }}
      </button>
    </section>
  </aside>
</template>

<script setup lang="ts">
import { defineProps, defineEmits, PropType, ref } from 'vue';

export type FilterOption = 'All' | 'Active' | 'Done';

const props = defineProps({
  activeFilter: {
    type: String as PropType<FilterOption>,
    required: true,
  },
});

const filters = ref<FilterOption[]>(['All', 'Active', 'Done']);

const emit = defineEmits(['changeFilter']);

const changeFilter = (newValue: FilterOption) => {
  emit('changeFilter', newValue);
};
</script>
