<template>
  <div class="container">
    <Header title="ToDo List" />
    <AddTask @add-task="addTask" />
    <Tasks @remind="remindTask" @delete-task="deleteTask" :tasks="tasks" />
  </div>
</template>

<script>
import Header from "./components/Header";
import Tasks from "./components/Tasks";
import AddTask from "./components/AddTask";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
      m: {},
      li1: [],
    };
  },
  methods: {
    addTask(task) {
      this.tasks.push(task);
      // console.log(JSON.stringify(task));
      fetch("http://localhost:3000/tasks", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(task),
      });
    },
    deleteTask(id) {
      if (confirm("Are you sure you want to delete this task?")) {
        this.tasks = this.tasks.filter((task) => task.id !== id);
        fetch("http://localhost:3000/tasks/" + id, {
          method: "DELETE",
        })
          .then((res) => res.json())
          .then((res) => console.log(res));
      }
    },
    async remindTask(id) {
      this.tasks = this.tasks.map((task) => {
        if (task.id === id) {
          task.reminder = !task.reminder;
        }
        return task;
      });

      const data = await fetch("http://localhost:3000/tasks/" + id).then(
        (response) => response.json()
      );
      const updTask = { ...data, reminder: !data.reminder };

      fetch("http://localhost:3000/tasks/" + id, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(updTask),
      });
    },
  },
  mounted() {
    fetch("http://localhost:3000/tasks")
      .then((response) => response.json())
      .then((data) => {
        this.tasks = data;
      });
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
