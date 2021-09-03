<template>
  <div class="home">
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Spinner v-if="loading" />
    <Tasks
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
  </div>
</template>

<script>
import Tasks from "../components/Tasks";
import AddTask from "../components/AddTask";
import Spinner from "../components/Spinner";

export default {
  name: "Home",
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask,
    Spinner,
  },
  data() {
    return {
      tasks: [],
      loading: false,
    };
  },
  methods: {
    async addTask(task) {
      const res = await fetch("https://zoesobol.pythonanywhere.com/tasks/", {
        method: "POST",
        headers: {
          "Access-Control-Allow-Origin": "*",
          "Content-Type": "application/json",
        },
        body: JSON.stringify(task),
      });
      //const newTask = await res.json();
      this.tasks.push(task);
    },
    async deleteTask(id) {
      if (confirm("Are you sure you want to delete this task?")) {
        const res = await fetch(
          `https://zoesobol.pythonanywhere.com/tasks/${id}`,
          {
            method: "DELETE",
            headers: {
              "Content-Type": "application/json",
            },
          }
        );
        res.status == 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("Error deleting task");
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      //const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const updTask = {
        text: taskToToggle.text,
        day: taskToToggle.day,
        reminder: !taskToToggle.reminder,
      };

      const data = await res.json();
      this.tasks = this.tasks.map((task) => {
        if (task.id === id) {
          data[task] = updTask;
        }
      });

      const res = await fetch(
        `https://zoesobol.pythonanywhere.com/tasks/${id}`,
        {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(this.tasks),
        }
      );
    },
    async fetchTasks() {
      const res = await fetch("https://zoesobol.pythonanywhere.com/tasks/");
      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      const res = await fetch(
        `https://zoesobol.pythonanywhere.com/tasks/${id}`,
        {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
          },
        }
      );
      const data = await res.json();
      return data;
    },
  },
  async created() {
    this.loading = true;
    this.tasks = await this.fetchTasks();
    this.loading = false;
  },
};
</script>
