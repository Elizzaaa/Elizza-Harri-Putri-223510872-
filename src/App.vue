<template>
  <div class="container">
    <div class="todo-app">

      <h1><center>List Kegiatan!</center></h1> <p> <h2><center>Masukkan Kegiatan yang kalian sukai</center></h2></p>
      <div class="row">
        <input type="text" v-model="newTask" placeholder="Masukkan">
        <button @click="addTask">ADD</button>
      </div>

      <div class="filters">
        <button @click="hideCompleted = !hideCompleted" class="button-74">

          {{ hideCompleted ? 'Tampilkan' : 'Sembunyikan' }}
        </button>
      </div>




      <ul>
        <li v-for="(task, index) in filteredTodos" :key="task.id" :class="{ checked: task.checked }" @click="toggleTask(task)">
          {{ task.text }}
          <span @click.stop="removeTask(task)">&#xd7;</span>
        </li>
      </ul>
    </div>


  </div>

</template>

<script setup>


import { ref, computed } from 'vue';

const newTask = ref('');

const tasks = ref([]);
const hideCompleted = ref(false);
const muted = ref(false);

const volume = ref(1);
const filteredTodos = computed(() => {
  return hideCompleted.value
    ? tasks.value.filter((task) => !task.checked)
    : tasks.value;
});
function addTask() {
  if (newTask.value.trim() === '') {
    alert("Isi terlebih dahulu!");
  } else {
    tasks.value.unshift({ id: Date.now(), text: newTask.value, checked: false });
    newTask.value = '';
    saveData();
  }
}
function toggleTask(task) {
  task.checked = !task.checked; 
  saveData();
}
function removeTask(task) {
  const index = tasks.value.findIndex((t) => t.id === task.id);
  if (index !== -1) {
    tasks.value.splice(index, 1);
    saveData();
  }
}
function saveData() {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
}
function loadData() {
  const savedTasks = localStorage.getItem("tasks");
  tasks.value = savedTasks ? JSON.parse(savedTasks) : [];
}
loadData();
</script>
<style scoped>
.container {
  background: rgb(239, 214, 176);
  width: 140%;
  min-height: 50vh;
  padding: 20px;
  margin-top: 10px;
  margin-left: 10px;
  margin-bottom: 10px;
}

.todo-app {
  width: 250%;
  max-width: 600px;
  background: rgb(175, 153, 143);
  margin: 0 auto 40px;
  padding: 10px 40px 70px;
}
.todo-app h1{
  text-align: right;
  color:rgb(236, 213, 213);
  margin-bottom: 30px;
  font-style: oblique;
}


input {
  flex: 6;
  border: 6px;
  outline: 3px;
  background: #dbdbc7;
  padding: 8px;
  font-size: 16px;
  font-weight: 200px;
  color: #000000;
}

.row button {
  outline: none;
  padding: 10px 10px;
  background: rgb(0, 0, 0);
  color: #fff;
  font-size: 10px;
  cursor: pointer;
}
.row button:hover{
  background:bottom;
}

ul li {
  list-style: none;
  font-size: 45px;
  padding: 10px 10px 4px 45px;
  user-select: none;
  cursor: pointer;
  position: relative;
  color:black;
}

ul li::before {
  content: '';
  position: absolute;
  height: 35px;
  width: 35px;
  border-radius: 20%;
  background-image: url(./assets/unchecked.png);
  background-size: cover;
  background-position: center;
  top: 30px;
  left: 5px;
}

ul li.checked {
  color:(205, 195, 160)ue;
  text-decoration: line-through;
  width: 5px;
}

ul li.checked::before {
  background-image: url(assets/checked.png);
}

ul li span {
  border-radius: 50%;
  position: absolute;
  right: 1;
  top: 30px;
  width: 35px;
  height: 35px;
  font-size: 35px;
  color: #e03a3a;
  line-height: 30px;
  text-align: center;
}

ul li span:hover {
  background: blue;
}

.filters{
display: flex;
justify-content: center;
padding-top: 5vh;
  
}

.filters button{
  width: 10px;
}


.button-74 {
  background-color: #000000;
  color: rgb(238, 188, 137);
  cursor: pointer;
  display: inline-block;
  font-weight: 500;
  font-size: 15px;
  padding: 0 60px;
  line-height: 40px;
  text-align: center;
  text-decoration: none;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

.button-74:hover {
  background-color: #424242;
}

@media (min-width: 800px) {
  .button-74 {
    min-width: 120px;
    padding: 0 35px;
  }
}
</style>
