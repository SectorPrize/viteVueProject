<script setup lang="ts">
import { ref, computed } from "vue";

let id;
const newItem = ref("");

const list = ref([]);

const storSize = window.localStorage.length;

console.log(localStorage);
//localStorage.clear()

if (storSize !== null) {
  id = Number(localStorage.getItem("index"));

  for (let i = 0; i <= id; i++) {
    if (localStorage.getItem("task" + i)) {
      list.value.push(JSON.parse(localStorage.getItem("task" + i)));
    }
  }
}

function addTask() {
  list.value.push({ id: id, title: newItem.value, status: false });
  localStorage.setItem(
    "task" + id,
    JSON.stringify({ id: id, title: newItem.value, status: false })
  );
  newItem.value = "";
  id++;
  localStorage.setItem("index", id.toString());
}

function clearTask() {
  newItem.value = "";
}

function removeTask(item) {
  list.value = list.value.filter((n) => n !== item);
  // console.log( item.id)
  localStorage.removeItem("task" + item.id);
}

function completeTask(item) {
  item.status = !item.status;
  newItem.value = "";
}

function editTask(item) {
  if (newItem.value == "") {
    newItem.value = item.title;
  } else {
    item.title = newItem.value;
  }
}
</script>

<template>
  <div class="task-list card-task">
    <div>
      <input
        id="vvod"
        v-model="newItem"
        required
        placeholder="Наименование задачи"
      />
      <button @click="addTask">Добавить</button>
      <button @click="clearTask">Очистить</button>
    </div>
    <div class="task">
      <ul>
        <li v-for="task in list" :key="task.id">
          <span :class="{ done: task.status }"
            >{{ task.title }} &nbsp;&nbsp;
          </span>
          <button @click="completeTask(task)">✔</button>
          <button @click="editTask(task)">✏️</button>
          <button @click="removeTask(task)">❌</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.card-task {
  justify-content: space-between;
  align-items: center;
  width: 500px;
}

.done {
  text-decoration: line-through;
}

.task {
  padding: 5px;
}

#vvod {
  padding-top: 10px;
}
</style>
