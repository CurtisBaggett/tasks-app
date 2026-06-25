<template>
  <button
    class="secondary"
    @click="emits('setFilter', filter)"
    :class="{
      contrast: currentFilter === filter,
    }"
  >
    {{ filter }}
  </button>
</template>

<script setup lang="ts">
import type { TaskFilter } from "../types";

const props = defineProps<{
  filter: TaskFilter;
  currentFilter: TaskFilter;
}>();

const emits = defineEmits<{
  setFilter: [filter: TaskFilter];
}>();
</script>

<style scoped>
button {
  text-transform: capitalize;
}
</style>
