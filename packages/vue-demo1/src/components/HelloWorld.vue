<template>
  <div>
    <p>Name: {{ name }}</p>
    <p>Age: {{ age }}</p>
    <button @click="sayHello">Say Hello</button>
  </div>
</template>

<script setup lang="ts">
import { name, age, sayHello } from 'shared-utils';
</script>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
