<template>
  <div :key="task.id" v-for="task in tasks">
    <Task :task="task" @delete-task="$emit('delete-task', task.id)" @toggle-remainder="$emit('toggle-remainder', task.id)" />
  </div>
</template>

<script>
import Task from './Task.vue';
export default {
  name: 'Tasks',
  props: {
    tasks: Array,
  },
  components: {
    Task,
  },
  emits: ['delete-task', 'toggle-remainder'],
};
</script>
