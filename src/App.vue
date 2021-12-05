<template>
 <h1>To-do List</h1>
 <div class="container">
  <h3 class="alret" v-if="alret">{{ alret }}</h3>
  <form @submit.prevent="submitForm">
   <input type="text" v-model="taskName" />
   <div>
    <button class="submit" v-if="!isEdited">Submit</button>
    <button class="submit" v-if="isEdited">Edit</button>
   </div>
  </form>
  <div class="tasks" v-if="tasks">
   <Tasks
    :tasksList="tasks"
    :isEdited="isEdited"
    @edit="editTask"
    @delete="deleteTask"
   />
  </div>

  <h3 class="no-tasks" v-if="isNoTasks">No Tasks!!</h3>
 </div>
</template>

<script>
import Tasks from "./components/Tasks.vue";
export default {
 name: "App",
 components: {
  Tasks,
 },
 data() {
  return {
   taskName: "",
   tasks: [],
   isEdited: false,
   editedTaskId: null,
   isNoTasks: false,
   alret: "",
  };
 },
 methods: {
  /* submit form */
  submitForm() {
   if (!this.taskName) {
    this.alret = "Task name can't be empty";
    this.hideAlret();

    return;
   }
   if (this.isEdited) {
    this.tasks.map((task) => {
     if (task.id === this.editedTaskId) {
      task.name = this.taskName;
     }
    });
    this.alret = "Task Edited!!";
    this.hideAlret();
   } else {
    this.tasks.push({
     id: new Date().getTime(),
     name: this.taskName,
    });
    this.alret = "New Task added!!";
    this.hideAlret();
   }
   localStorage.setItem("tasks", JSON.stringify(this.tasks));
   this.taskName = "";
   this.isEdited = false;
   this.isNoTasks = false;
  },

  /* edit task */
  editTask(id) {
   const editedTask = this.tasks.find((task) => task.id === id);
   this.taskName = editedTask.name;
   this.isEdited = true;
   this.editedTaskId = id;
  },

  /* delete task */
  deleteTask(id) {
   const filtredTasks = this.tasks.filter((task) => task.id !== id);
   localStorage.setItem("tasks", JSON.stringify(filtredTasks));
   this.tasks = filtredTasks;
   this.hideAlret();

   if (this.tasks.length == 0) {
    this.isNoTasks = true;
   }
   this.alret = "Task Deleted!!";
  },

  /* hide alret */
  hideAlret() {
   setTimeout(() => {
    this.alret = false;
   }, 3000);
  },
 },
 beforeMount() {
  this.tasks = JSON.parse(localStorage.getItem("tasks"))
   ? JSON.parse(localStorage.getItem("tasks"))
   : [];
 },
};
</script>

<style>
#app {
 font-family: Avenir, Helvetica, Arial, sans-serif;
 -webkit-font-smoothing: antialiased;
 -moz-osx-font-smoothing: grayscale;
 text-align: center;
 color: #2c3e50;
}

.container {
 width: 400px;
 box-shadow: 0 0 10px #ddd;
 margin: 0 auto;
 padding: 4rem 1rem 1rem 1rem;
 border-radius: 10px;
 position: relative;
}

form {
 display: flex;
 justify-content: center;
 align-items: center;
 margin-bottom: 2rem;
}

input {
 border: 2px solid #ccc;
 margin-right: 10px;
}

button.submit {
 width: 80px;
 border: 2px solid rgb(58, 58, 243);
 background-color: #fff;
 transition: all 0.3s ease;
 cursor: pointer;
}

input,
button {
 font-size: 18px;
 padding: 0.5rem;
 border-radius: 5px;
}

button.submit:hover {
 background-color: rgb(58, 58, 243);
 color: #fff;
}

.alret {
 position: absolute;
 width: 100%;
 top: 10px;
 left: 50%;
 transform: translate(-50%, -50%);
}
</style>
