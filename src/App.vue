<template>
  <main>
    <h1>{{ message }}</h1>
    <TaskForm @add-task="addTask" />
    <h3 v-if="!tasks.length">Add a task to get started</h3>
    <h3 v-else>{{ totalDone }} / {{ tasks.length }} completed</h3>
    <div v-if="tasks.length" class="button-container">
      <FilterButton
        :currentFilter="filter"
        filter="all"
        @set-filter="setFilter"
      />
      <FilterButton
        :currentFilter="filter"
        filter="todo"
        @set-filter="setFilter"
      />
      <FilterButton
        :currentFilter="filter"
        filter="done"
        @set-filter="setFilter"
      />
    </div>
    <TaskList
      :tasks="filteredTasked"
      @toggle-done="toggleDone"
      @remove-task="removeTask"
    />
  </main>
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from "vue";
import TaskForm from "./components/TaskForm.vue";
import TaskList from "./components/TaskList.vue";
import type { Task, TaskFilter } from "./types.ts";
import FilterButton from "./components/FilterButton.vue";

const message = ref("Tasks App");
const tasks = ref<Task[]>([]);
const filter = ref<TaskFilter>("all");

onMounted(() => {
  const savedTasks = JSON.parse(localStorage.getItem("tasks") || "[]");

  if (savedTasks) {
    tasks.value = savedTasks;
  }
  console.log(savedTasks);
});

const totalDone = computed(() =>
  tasks.value.reduce((total, task) => (task.done ? total + 1 : total), 0),
);

const filteredTasked = computed(() => {
  switch (filter.value) {
    case "all":
      return tasks.value;
    case "done":
      return tasks.value.filter((task) => task.done);
    case "todo":
      return tasks.value.filter((task) => !task.done);
  }
});

const addTask = (newTask: string) => {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false,
  });

  saveTaskToLocalStorage();
};

const toggleDone = (id: string) => {
  const task = tasks.value.find((task) => task.id === id);

  if (task) {
    task.done = !task.done;
  }

  saveTaskToLocalStorage();
};

const removeTask = (id: string) => {
  const index = tasks.value.findIndex((task) => task.id === id);
  if (index !== -1) {
    tasks.value.splice(index, 1);
  }

  saveTaskToLocalStorage();
};

const setFilter = (value: TaskFilter) => {
  filter.value = value;
};

const saveTaskToLocalStorage = () => {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
  console.log(tasks.value);
};
</script>

<style>
main {
  max-width: 800px;
  margin: 1rem auto;
}

.button-container {
  display: flex;
  justify-content: end;
  gap: 0.5rem;
}
</style>
