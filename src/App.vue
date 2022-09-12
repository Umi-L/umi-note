<script lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import Sidebar from "./components/Sidebar.vue";
import NoteArea from "./components/NoteArea.vue";
import { defineComponent } from "vue";

export default defineComponent({
  components: {
    Sidebar,
    NoteArea,
  },
  methods: {
    open_file(file: string) {
      console.log(file);
      this.$refs.NoteArea.open_file(file);
    },
  },
});
</script>


<template>
  <sidebar @open_file="open_file"></sidebar>
  <NoteArea ref="NoteArea"></NoteArea>
</template>

<style>
#app {
  height: 100vh;
}
</style>
