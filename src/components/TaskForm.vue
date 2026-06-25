<template>
  <form @submit.prevent="formSubmitted">
    <label>
      New Task
      <input
        v-model="newTask"
        type="text"
        name="newTask"
        :aria-invalid="!!error || undefined"
        @input="error = ''"
      />
      <small v-if="error" id="invalid-helper"> {{ error }} </small>
    </label>
    <div class="button-container">
      <button>Add</button>
    </div>
  </form>
</template>

<script setup lang="ts">
import { ref } from "vue";

const emit = defineEmits<{
  addTask: [newTask: string];
}>();

const newTask = ref("");
const error = ref("");

const formSubmitted = () => {
  if (newTask.value.trim()) {
    emit("addTask", newTask.value.trim());
    newTask.value = "";
  } else {
    error.value = "Task cannot be empty!";
  }
};
</script>
