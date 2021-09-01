<template #default>
  <div>
    <div :key="task.id" v-for="task in tasks">
      <Task
        @toggle-reminder="$emit('toggle-reminder', task.id)"
        @delete-task="$emit('delete-task', task.id)"
        :task="task"
      />
    </div>
  </div>
</template>

<script>
import Task from "./Task";

export default {
  name: "Tasks",
  data() {
    return {
      componentKey: 0,
    };
  },
  props: {
    tasks: Array,
  },
  components: {
    Task,
  },
  methods: {
    forceRerender() {
      this.componentKey += 1;
    },
  },
  emits: ["delete-task", "toggle-reminder"],
};
</script>
