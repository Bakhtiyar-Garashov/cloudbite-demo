<script setup>
import TodoItem from "./components/TodoItem.vue";
import { reactive, ref, onMounted } from "vue";
import firebase from "@/firebaseInit";

const db = firebase.collection("/todos");

const todo = reactive({
  text: "",
});

let todos = ref([]);

const addTodo = () => {
  if (todo.text.length > 3) {
    db.add({
      text: todo.text,
      createdAt: new Date(),
    })
      .then(() => {
        todo.text = "";
      })
      .catch((err) => {
        console.log(err);
      });
    getTodos();
  } else {
    alert("Please enter a longer todo (>3 characters)");
  }
};

const getTodos = async () => {
  todos.value = [];
  const snapshot = await db.orderBy("createdAt", "asc").get();
  snapshot.forEach((doc) => {
    todos.value.push(doc.data());
  });
};

onMounted(() => {
  getTodos();
});
</script>

<template>
  <div class="add-todo">
    <input type="text" v-model="todo.text" @keyup.enter="addTodo" />
    <button @click="addTodo">Add Todo</button>
  </div>
  <div class="todos-list">
    <TodoItem v-for="(todo, index) in todos" :todo="todo" :key="index" />
  </div>
</template>

<style>
.add-todo {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
}

.add-todo input {
  width: 50%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.add-todo button {
  margin-top: 20px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: #eee;
}
</style>
