<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import Sidebar from "./components/Sidebar.vue";
import NoteArea from "./components/NoteArea.vue";
</script>


<template>
  <sidebar></sidebar>
  <NoteArea></NoteArea>
</template>

<style>
#app {
  height: 100vh;
}
</style>
