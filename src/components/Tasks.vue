// eslint-disable-next-line no-unused-vars
<template>
  <div v-for="task in tasks" :key="task.id">
    <Task
      @remind="$emit('remind', task.id)"
      @delete-task="$emit('delete-task', task.id)"
      :task="task"
    />
  </div>
</template>

<script>
import Task from "./Task.vue";

export default {
  name: "Tasks",
  components: {
    Task,
  },
  props: {
    tasks: Array,
  },
  emits: ["delete-task", "remind"],
};
</script>