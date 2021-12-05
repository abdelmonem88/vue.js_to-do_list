<template>
 <div class="task" v-for="task in tasksList" :key="task.id">
  <h3 class="task_name">{{ task.name }}</h3>
  <div class="task_btns">
   <button
    :disabled="isEdited"
    class="edit-btn"
    @click="$emit('edit', task.id)"
   >
    Edit
   </button>
   <button class="edit-delete" @click="$emit('delete', task.id)">Delete</button>
  </div>
 </div>
</template>

<script>
export default {
 name: "Tasks",
 props: {
  tasksList: Array,
  isEdited: Boolean,
 },
 methods: {
  editTask(id) {
   console.log(id);
  },
  deleteTask(id) {
   let newTasks = this.tasksList.filter((task) => task.id !== id);
   localStorage.setItem("tasks", JSON.stringify(newTasks));
  },
 },
 emits: ["edit", "delete"],
};
</script>

<style scoped>
.task {
 box-shadow: 0 0 10px #ddd;
 padding: 0.5rem 1rem;
 display: flex;
 justify-content: space-between;
 align-items: center;
 margin-bottom: 1rem;
 font-size: 14px;
}

.task_name {
 text-transform: capitalize;
}

.task_btns button {
 width: 60px;
 padding: 0.25rem 0;
 font-size: 14px;
 background-color: #fff;
 border: 1px solid;
 border-radius: 5px;
 transition: all 0.3s ease;
 cursor: pointer;
}

.task_btns button:first-child {
 margin-right: 0.5rem;
 border-color: rgb(93, 185, 1);
}

.task_btns button:first-child:hover {
 background-color: rgb(93, 185, 1);
 color: #fff;
}
.task_btns button:nth-child(2) {
 border-color: rgb(253, 37, 37);
}

.task_btns button:nth-child(2):hover {
 background-color: rgb(253, 37, 37);
 color: #fff;
}
</style>
