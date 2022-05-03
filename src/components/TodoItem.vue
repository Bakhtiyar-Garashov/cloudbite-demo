<script setup>
defineProps({ todo: { type: Object } });
</script>

<template>
  <div class="todos-item">
    <div class="each-todo">{{ todo.text }}</div>
  </div>
</template>

<style>
.todos-item {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
}

.each-todo {
  margin: 0 auto;
  text-align: start;
}
</style>
